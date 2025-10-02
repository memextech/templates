# Full Stack App Implementation Guide

## Overview
A production-ready full-stack application with email OTP authentication, built with Next.js 15, Netlify Functions, and Turso database.

## Tech Stack
- **Frontend**: Next.js 15 (App Router) + TypeScript + Tailwind CSS
- **Backend**: Netlify Functions (TypeScript with esbuild)
- **Database**: Turso (SQLite) with Drizzle ORM
- **Authentication**: Email OTP via Resend
- **Hosting**: Netlify

## Implementation Order

### Phase 1: App Name
1. If user did not specify explicitly ask user for App Name
2. Use this name across this guide
3. If user does not specify explicitly ask what Type of application it is
4. Database design and pages in this guide are for todo app, ammend the implementation accordingly to the user stype of application ensuring you follow closly all design and technology choices in this guide.

### Phase 2: Local Development Setup
Complete all local setup before any cloud deployment:
1. Install dependencies and global tools
2. Ensure required ports are available and kill processes if necessary
3. Setup Turso local development server
4. Initialize database schema
5. Create all components and API functions
6. Use Tailwind CSS to make the app look stunning (ensure textfield hints have the right color)
7. Configure Netlify development environment
8. Test authentication flow locally (OTP codes in console)
9. Test CRUD operations locally
10. Verify app works at `http://localhost:8888`
11. Ask user to verify app works at `http://localhost:8888` and tell user how to find OTP codes in console

### Phase 3: Production Deployment
Only after local setup is fully working and verified by user:
1. Create NEW Turso cloud database (separate from dev.db) using provided App Name
2. Generate NEW production auth tokens
3. Retrieve production secrets (RESEND_API_KEY)
4. Create NEW Netlify project using provided App Name
5. Deploy with production environment variables
6. Test production endpoints

## Installation

### Dependencies
```bash
# Create Next.js app with TypeScript and Tailwind
npx create-next-app@latest . --typescript --tailwind --eslint --app --src-dir --import-alias "@/*"

# Add required packages
npm install @libsql/client drizzle-orm @netlify/functions uuid @types/uuid dotenv
npm install -D drizzle-kit

# Global tools
npm install -g netlify-cli
brew install tursodatabase/tap/turso  # Alternative: npm install -g @turso/cli
```

## Project Structure
```
├── src/
│   ├── app/
│   │   └── page.tsx
│   ├── components/
│   │   ├── TodoApp.tsx
│   │   └── EmailOTPAuth.tsx
│   └── lib/
│       ├── db.ts
│       ├── schema.ts
│       └── auth-client.ts
├── netlify/
│   └── functions/
│       ├── send-otp.ts
│       ├── verify-otp.ts
│       ├── session.ts
│       ├── todos.ts
│       └── todos-id.ts
├── drizzle.config.ts
├── next.config.ts
├── netlify.toml
├── start.sh
└── .env.local
```

## Configuration Files

### next.config.ts
```typescript
import type { NextConfig } from "next";

const nextConfig: NextConfig = {
  webpack: (config, { isServer, webpack }) => {
    if (!isServer) {
      config.resolve.fallback = {
        fs: false,
        net: false,
        tls: false,
        crypto: false,
        path: false,
        stream: false,
      };
      config.plugins.push(
        new webpack.IgnorePlugin({
          resourceRegExp: /@libsql\/client|drizzle-orm|better-sqlite3/,
        })
      );
    }
    return config;
  },
  serverExternalPackages: ['@libsql/client', 'drizzle-orm', 'better-sqlite3']
};

export default nextConfig;
```

### netlify.toml
```toml
[functions]
  directory = "netlify/functions"
  node_bundler = "esbuild"

[[redirects]]
  from = "/api/auth/send-otp"
  to = "/.netlify/functions/send-otp"
  status = 200

[[redirects]]
  from = "/api/auth/verify-otp"
  to = "/.netlify/functions/verify-otp"
  status = 200

[[redirects]]
  from = "/api/auth/session"
  to = "/.netlify/functions/session"
  status = 200

[[redirects]]
  from = "/api/todos"
  to = "/.netlify/functions/todos"
  status = 200

[[redirects]]
  from = "/api/todos/*"
  to = "/.netlify/functions/todos-id"
  status = 200
```

### drizzle.config.ts
```typescript
import type { Config } from 'drizzle-kit';
import * as dotenv from 'dotenv';

dotenv.config({ path: '.env.local' });

const url = process.env.TURSO_DATABASE_URL || 'http://127.0.0.1:8080';
const authToken = process.env.TURSO_AUTH_TOKEN;

const config: Config = {
  schema: './src/lib/schema.ts',
  out: './drizzle',
  dialect: 'sqlite',
  dbCredentials: authToken ? { url, authToken } : { url },
} as Config;

export default config;
```

### tsconfig.json
Ensure strict TypeScript configuration for production builds:
```json
{
  "compilerOptions": {
    "lib": ["dom", "dom.iterable", "esnext"],
    "allowJs": true,
    "skipLibCheck": true,
    "strict": true,
    "noEmit": true,
    "esModuleInterop": true,
    "module": "esnext",
    "moduleResolution": "bundler",
    "resolveJsonModule": true,
    "isolatedModules": true,
    "jsx": "preserve",
    "incremental": true,
    "plugins": [
      {
        "name": "next"
      }
    ],
    "paths": {
      "@/*": ["./src/*"]
    }
  },
  "include": ["next-env.d.ts", "**/*.ts", "**/*.tsx", ".next/types/**/*.ts"],
  "exclude": ["node_modules"]
}
```

## Database Schema

### src/lib/schema.ts
```typescript
import { sqliteTable, text, integer } from 'drizzle-orm/sqlite-core';
import { relations } from 'drizzle-orm';

export const users = sqliteTable('user', {
  id: text('id').primaryKey(),
  name: text('name'),
  email: text('email').notNull().unique(),
  emailVerified: integer('emailVerified', { mode: 'boolean' }).notNull().default(false),
  image: text('image'),
  createdAt: text('createdAt').notNull(),
  updatedAt: text('updatedAt').notNull(),
});

export const sessions = sqliteTable('session', {
  id: text('id').primaryKey(),
  expiresAt: text('expiresAt').notNull(),
  token: text('token').notNull().unique(),
  createdAt: text('createdAt').notNull(),
  updatedAt: text('updatedAt').notNull(),
  ipAddress: text('ipAddress'),
  userAgent: text('userAgent'),
  userId: text('userId').notNull().references(() => users.id, { onDelete: 'cascade' }),
});

export const verifications = sqliteTable('verification', {
  id: text('id').primaryKey(),
  identifier: text('identifier').notNull(),
  value: text('value').notNull(),
  expiresAt: text('expiresAt').notNull(),
  createdAt: text('createdAt').notNull(),
  updatedAt: text('updatedAt').notNull(),
});

export const accounts = sqliteTable('account', {
  id: text('id').primaryKey(),
  userId: text('userId').notNull().references(() => users.id, { onDelete: 'cascade' }),
  accountId: text('accountId').notNull(),
  providerId: text('providerId').notNull(),
  accessToken: text('accessToken'),
  refreshToken: text('refreshToken'),
  expiresAt: text('expiresAt'),
  createdAt: text('createdAt').notNull(),
  updatedAt: text('updatedAt').notNull(),
});

export const todos = sqliteTable('todos', {
  id: text('id').primaryKey(),
  title: text('title').notNull(),
  description: text('description'),
  completed: integer('completed', { mode: 'boolean' }).notNull().default(false),
  createdAt: integer('createdAt', { mode: 'timestamp' }).notNull(),
  updatedAt: integer('updatedAt', { mode: 'timestamp' }).notNull(),
  userId: text('userId').notNull().references(() => users.id, { onDelete: 'cascade' }),
});

// Relations
export const usersRelations = relations(users, ({ many }) => ({
  sessions: many(sessions),
  todos: many(todos),
}));

export const todosRelations = relations(todos, ({ one }) => ({
  user: one(users, {
    fields: [todos.userId],
    references: [users.id],
  }),
}));
```

## Critical: Drizzle Database Connection

### ⚠️ MUST use sqlite-proxy adapter for Netlify Functions
The standard Drizzle setup with `@libsql/client` fails in AWS Lambda. Always use this pattern:

```typescript
import { drizzle } from 'drizzle-orm/sqlite-proxy';
import { createClient } from '@libsql/client/http';

const client = createClient({
  url: process.env.TURSO_DATABASE_URL || '',
  authToken: process.env.TURSO_AUTH_TOKEN
});

const db = drizzle(async (sql, params) => {
  const result = await client.execute({ sql, args: params });
  return { rows: result.rows };
});
```

## Netlify Functions Template

### Standard Function Pattern
Every Netlify function should follow this structure with proper TypeScript types:

```typescript
import { drizzle } from 'drizzle-orm/sqlite-proxy';
import { createClient } from '@libsql/client/http';
import { users } from '../../src/lib/schema';
import { eq } from 'drizzle-orm';
import type { Handler, HandlerEvent, HandlerContext } from '@netlify/functions';

export const handler: Handler = async (event: HandlerEvent, context: HandlerContext) => {
  const headers = {
    'Access-Control-Allow-Origin': '*',
    'Access-Control-Allow-Headers': 'Content-Type, Authorization',
    'Access-Control-Allow-Methods': 'GET, POST, OPTIONS',
    'Content-Type': 'application/json',
  };

  // Handle CORS preflight
  if (event.httpMethod === 'OPTIONS') {
    return { statusCode: 200, headers, body: '' };
  }

  try {
    // Database connection (MUST use sqlite-proxy)
    const client = createClient({
      url: process.env.TURSO_DATABASE_URL || '',
      authToken: process.env.TURSO_AUTH_TOKEN
    });

    const db = drizzle(async (sql, params) => {
      const result = await client.execute({ sql, args: params });
      return { rows: result.rows };
    });

    // Your business logic here
    const result = await db.select().from(users);

    return {
      statusCode: 200,
      headers,
      body: JSON.stringify(result)
    };
  } catch (error) {
    console.error('Error:', error);
    return {
      statusCode: 500,
      headers,
      body: JSON.stringify({ error: 'Internal server error' })
    };
  }
};
```

## Component Implementation Guidelines

### TypeScript Best Practices
To avoid lint errors during production deployment:

1. **Always use proper type annotations**
```typescript
// ❌ Bad - will cause lint errors
const handleSubmit = (e) => { ... }

// ✅ Good
const handleSubmit = (e: React.FormEvent) => { ... }
```

2. **Avoid any type**
```typescript
// ❌ Bad
const data: any = await response.json();

// ✅ Good
interface ApiResponse {
  success: boolean;
  message?: string;
  token?: string;
}
const data: ApiResponse = await response.json();
```

3. **Use proper async/await error handling**
```typescript
// ✅ Always handle errors in async functions
try {
  const result = await apiCall();
} catch (error) {
  console.error('Error:', error);
  // Handle error appropriately
}
```

### Login Page Implementation (src/app/page.tsx)
**Important**: Use replace() instead of forward() to avoid navigation issues:

```typescript
'use client';

import { useEffect, useState } from 'react';
import { useRouter } from 'next/navigation';
import EmailOTPAuth from '@/components/EmailOTPAuth';
import TodoApp from '@/components/TodoApp';
import authClient from '@/lib/auth-client';

export default function Home() {
  const router = useRouter();
  const { data: session, isPending } = authClient.useSession();
  const [isAuthenticated, setIsAuthenticated] = useState(false);

  useEffect(() => {
    if (!isPending) {
      if (session) {
        setIsAuthenticated(true);
      } else {
        setIsAuthenticated(false);
      }
    }
  }, [session, isPending]);

  const handleSignInSuccess = () => {
    setIsAuthenticated(true);
    // Use replace() NOT forward() to avoid navigation issues
    router.replace('/');
  };

  const handleSignOut = async () => {
    try {
      await authClient.signOut();
      setIsAuthenticated(false);
      router.replace('/');
    } catch (error) {
      console.error('Sign out error:', error);
    }
  };

  if (isPending) {
    return <div>Loading...</div>;
  }

  if (!isAuthenticated) {
    return <EmailOTPAuth onSuccess={handleSignInSuccess} />;
  }

  return <TodoApp onSignOut={handleSignOut} />;
}
```

## Authentication Implementation

### Email OTP with Resend
- Use sandbox domain `onboarding@resend.dev` (no verification needed)
- OTP codes: 6 digits, 5-minute expiry
- Session tokens: 64-char hex, 7-day expiry
- In development: OTP codes appear in console

### Send OTP Email
```typescript
await fetch('https://api.resend.com/emails', {
  method: 'POST',
  headers: {
    'Authorization': `Bearer ${process.env.RESEND_API_KEY}`,
    'Content-Type': 'application/json',
  },
  body: JSON.stringify({
    from: 'onboarding@resend.dev',
    to: email,
    subject: `${otpCode} is your verification code`,
    html: emailHtml,
  }),
});
```

### Auth Client Structure (src/lib/auth-client.ts)
Export as plain object with hooks:
```typescript
const authClient = {
  signIn: { emailOTP: signInEmailOTP },
  verifyOTP,
  signOut,
  useSession  // Must be standalone function, not method
};
```

## Environment Variables

### Local Development (.env.local)
```
TURSO_DATABASE_URL="http://127.0.0.1:8080"
BETTER_AUTH_SECRET="min-32-chars"
NEXT_PUBLIC_APP_URL="http://localhost:8888"
NODE_ENV="development"
```

### Production (Netlify Environment)
```
TURSO_DATABASE_URL="libsql://your-db.turso.io"
TURSO_AUTH_TOKEN="eyJ..."
RESEND_API_KEY="re_..."
BETTER_AUTH_SECRET="min-32-chars"
NEXT_PUBLIC_APP_URL="https://your-app.netlify.app"
NODE_ENV="production"
```

## Local Development Script

### start.sh
```bash
#!/bin/bash
cd "$(dirname "$0")"

# Kill any existing processes on required ports
echo "Cleaning up existing processes..."
lsof -ti:8080 | xargs kill -9 2>/dev/null || true
lsof -ti:8888 | xargs kill -9 2>/dev/null || true
lsof -ti:3000 | xargs kill -9 2>/dev/null || true
sleep 1

# Start Turso dev server
echo "Starting Turso dev server..."
turso dev --db-file dev.db > /dev/null 2>&1 &
TURSO_PID=$!
sleep 2

# Initialize database schema once
if [ ! -f ".db_initialized" ]; then
  echo "Setting up database schema..."
  npm run db:push
  touch .db_initialized
fi

echo "Starting Netlify dev server..."
npm run netlify:dev

trap "kill $TURSO_PID 2>/dev/null" EXIT
```

## Production Deployment Checklist

### Prerequisites
- Local development fully working
- Access to keyring CLI for secrets

### Deployment Steps

#### 1. Create Production Database
```bash
# Create NEW production database
turso db create your-app-name

# Get database URL
turso db show your-app-name --url

# Generate NEW auth token
turso db tokens create your-app-name

# Apply schema
turso db shell your-app-name < drizzle/0000_*.sql
```

#### 2. Retrieve Production Secrets
```bash
# Install keyring if needed
pip install keyring  # or: uv tool install keyring

# Get RESEND API key
keyring get memex RESEND_API_KEY
```

#### 3. Initialize Netlify Project
```bash
# Create NEW Netlify project
netlify init

# Choose: Create and deploy manually (without git)
# Build command: npm run build
# Directory to deploy: .next
```

#### 4. Configure Production Environment
```bash
# Set all environment variables in one command
netlify env:set TURSO_DATABASE_URL "libsql://your-app-name.turso.io" && \
netlify env:set TURSO_AUTH_TOKEN "eyJ..." && \
netlify env:set RESEND_API_KEY "re_..." && \
netlify env:set BETTER_AUTH_SECRET "min-32-chars-random" && \
netlify env:set NEXT_PUBLIC_APP_URL "https://your-app.netlify.app" && \
netlify env:set NODE_ENV "production"
```

#### 5. Deploy to Production
```bash
# Build locally first
npm run build

# Deploy to production
netlify deploy --prod --skip-functions-cache
```

#### 6. Verify Deployment
```bash
# Test OTP sending
curl -X POST https://your-app.netlify.app/api/auth/send-otp \
  -H "Content-Type: application/json" \
  -d '{"email":"delivered@resend.dev"}'

# Expected: {"success":true,"message":"OTP sent to your email"}
```

## Testing API Endpoints

### Authentication Flow
```bash
# 1. Send OTP
curl -X POST http://localhost:8888/api/auth/send-otp \
  -H "Content-Type: application/json" \
  -d '{"email":"test@example.com"}'

# 2. Verify OTP (check console for code in dev)
curl -X POST http://localhost:8888/api/auth/verify-otp \
  -H "Content-Type: application/json" \
  -d '{"email":"test@example.com","otp":"123456"}'

# 3. Check session
curl http://localhost:8888/api/auth/session \
  -H "Authorization: Bearer YOUR_TOKEN"
```

### Todo CRUD Operations
```bash
# Create todo
curl -X POST http://localhost:8888/api/todos \
  -H "Authorization: Bearer YOUR_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{"title":"Test Todo","description":"Testing"}'

# Get todos
curl http://localhost:8888/api/todos \
  -H "Authorization: Bearer YOUR_TOKEN"

# Update todo
curl -X PUT http://localhost:8888/api/todos/TODO_ID \
  -H "Authorization: Bearer YOUR_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{"completed":true}'

# Delete todo
curl -X DELETE http://localhost:8888/api/todos/TODO_ID \
  -H "Authorization: Bearer YOUR_TOKEN"
```

## Common Issues & Solutions

### Issue: Netlify Function fails with native module error
**Solution**: Always use `drizzle-orm/sqlite-proxy` with HTTP client, never `drizzle-orm/libsql`

### Issue: OTP emails not sending in development
**Solution**: Check console output - OTP codes are displayed there in development mode

### Issue: Database connection fails in production
**Solution**: Ensure you created a NEW Turso database and token for production, not reusing dev credentials

### Issue: CORS errors in browser
**Solution**: Verify all Netlify functions include proper CORS headers and handle OPTIONS requests

### Issue: Port already in use errors
**Solution**: The start.sh script now kills existing processes. If issues persist:
```bash
# Manually kill processes
lsof -ti:8080 | xargs kill -9
lsof -ti:8888 | xargs kill -9
lsof -ti:3000 | xargs kill -9
```

### Issue: TypeScript errors during production build
**Solution**: Follow these guidelines:
1. Never use `any` type without explicit reason
2. Always type event handlers: `(e: React.FormEvent)`
3. Define interfaces for API responses
4. Use proper async error handling
5. Import types explicitly: `import type { Handler } from '@netlify/functions'`

### Issue: Navigation issues after login
**Solution**: Use `router.replace('/')` instead of `router.forward()` or `router.push()`

## Key Implementation Rules

1. **Database Connection**: ALWAYS use sqlite-proxy adapter in Netlify Functions
2. **HTTP Client Only**: Use `@libsql/client/http` to avoid native dependencies
3. **Development Mode**: OTP codes display in console when NODE_ENV=development
4. **Email Testing**: Use `onboarding@resend.dev` for immediate testing
5. **Production Isolation**: NEVER reuse development database or tokens in production
6. **Error Handling**: Always wrap database operations in try-catch blocks
7. **CORS Support**: Every function must handle OPTIONS requests and include CORS headers

## Package.json Scripts
```json
{
  "scripts": {
    "dev": "next dev",
    "build": "next build",
    "start": "next start",
    "netlify:dev": "netlify dev",
    "db:push": "drizzle-kit push",
    "db:studio": "drizzle-kit studio"
  }
}
```