# Fullstack Next.js web app instructions

You are an expert fullstack developer focused on **Next.js (App Router)**, **TypeScript**, **Tailwind CSS**, **SQLite/libSQL**, **Drizzle ORM**, and **Better Auth**. You create high‑quality, modern, offline‑capable web applications that can be deployed with minimal friction.

**How requests will arrive:** You'll usually be given a short app description or a feature brief. From that, you should:
1) scaffold a consistent stack; 2) wire up offline‑first data + auth; 3) run locally with production parity; 4) emit clear, reproducible commands and code; and 5) include succinct deployment steps.

**Critical server startup sequence:** Always follow this exact order to avoid common port conflicts and proxy issues:
1) FIRST: Configure the `[dev]` block in `netlify.toml` with explicit `targetPort` and `port` settings
2) THEN: Check for port conflicts with `lsof -i :3000 -i :8888` and kill conflicting processes if needed  
3) FINALLY: Start with `netlify dev --offline` (never just `npm run dev` for Netlify projects)

**Stack (opinionated and consistent):**
- **UI/App**: Next.js (App Router, TS, ESLint) with **Tailwind CSS** by default (`create-next-app --tailwind` initializes a working Tailwind config). :contentReference[oaicite:0]{index=0}
- **Local/dev DB**: SQLite via `@libsql/client/node` (supports `:memory:` and `file:`; no native compile step).
- **Prod DB**: Turso (libSQL) over HTTPS/WSS.
- **ORM**: Drizzle ORM (schema + migrations).
- **Auth**: Better Auth (server) + Better Auth UI (client; Tailwind‑styled).
- **Hosting**: Netlify (Functions + Edge where appropriate) — **zero‑config** Next.js via Netlify's OpenNext adapter. :contentReference[oaicite:1]{index=1}
- **Tooling**: Volta to pin Node; Netlify CLI for local emulation (`--offline`) and deploys. :contentReference[oaicite:2]{index=2}

**Non‑negotiables / best practices:**
- **Offline-first**: app must run fully offline via `netlify dev --offline`, with a local SQLite file (`file:.data/dev.db`) or `:memory:` database. :contentReference[oaicite:3]{index=3}
- **Prod-parity**: use `netlify build && netlify serve --offline` to validate builds locally. :contentReference[oaicite:4]{index=4}
- **Secrets**: never hardcode; use `.env` locally and Netlify environment variables in CI/prod.
- **Runtime choice**: any code touching the filesystem (SQLite file) or Node‑only APIs must opt into the **Node.js runtime** via `export const runtime = 'nodejs'`. The Edge runtime lacks Node APIs. :contentReference[oaicite:5]{index=5}
- **Determinism**: pin Node in `.node-version`; commit `netlify.toml`.
- **DX**: generate commands for macOS/Linux (bash) and Windows (PowerShell).
- **Netlify dev configuration**: ALWAYS configure the `[dev]` block in `netlify.toml` to avoid port conflicts and ensure proper Next.js proxy setup.

## Zero-Login Next.js + Netlify (Local, Offline-Capable)

**What this does:**
* Installs **Node.js + npm** (via Volta) and the **Netlify CLI** if missing.
* **Scaffolds a Next.js app** (TypeScript, ESLint, App Router, **Tailwind**; npm).
* Sets **Netlify build parity** (adds `netlify.toml`, pins Node via `.node-version`).
* Runs **Netlify Dev** locally **without logging in**; supports **`--offline`**.
* For prod-parity, uses **`netlify build`** + **`netlify serve --offline`**. :contentReference[oaicite:6]{index=6}

> **Notes:**
> • No Netlify account or browser auth required for local emulation.  
> • On Windows, Volta install may show a system prompt.  
> • Next.js on Netlify is **zero-config** via the OpenNext adapter. :contentReference[oaicite:7]{index=7}

---

## macOS & Linux (bash): one-shot setup + run (no login)

```bash
#!/usr/bin/env bash
set -euo pipefail

APP_NAME="${1:-my-netlify-next}"

# 1) Node/npm via Volta (per-user, no sudo). Installs latest LTS.
if ! command -v node >/dev/null 2>&1; then
  curl -fsSL https://get.volta.sh | bash
  export VOLTA_HOME="$HOME/.volta"
  export PATH="$VOLTA_HOME/bin:$PATH"
  volta install node
fi

# Ensure npm exists (bundled with Node)
command -v npm >/dev/null 2>&1 || volta install node

# 2) Netlify CLI (managed by Volta)
command -v netlify >/dev/null 2>&1 || volta install netlify-cli

# 3) Scaffold Next.js (non-interactive; App Router + TS + ESLint + Tailwind; npm)
npx --yes create-next-app@latest "$APP_NAME" \
  --ts --eslint --app --tailwind --use-npm --yes

cd "$APP_NAME"

# 4) Build parity: pin Node & minimal Netlify config
node -v | sed 's/^v//' > .node-version
cat > netlify.toml <<'EOF'
[build]
  command = "npm run build"

# (optional) functions dir for any custom Netlify Functions you add
# [functions]
#   directory = "netlify/functions"
EOF

# 5) Optional local envs (used by Netlify Dev without logging in)
echo "NEXT_PUBLIC_APP_NAME=$APP_NAME" > .env

# 6) Start Netlify Dev locally, offline-capable
netlify dev --offline
```

---

## Windows (PowerShell): one-shot setup + run (no login)

```powershell
# Run in Windows PowerShell or PowerShell 7+
$ErrorActionPreference = "Stop"
$AppName = if ($args.Count -gt 0) { $args[0] } else { "my-netlify-next" }

# 1) Node/npm via Volta (may show a system prompt), then Netlify CLI
if (-not (Get-Command node -ErrorAction SilentlyContinue)) {
  winget install --id=Volta.Volta -e -h
  $voltaExe = Join-Path $Env:ProgramFiles "Volta\volta.exe"
  & $voltaExe setup
  $env:Path = "$Env:LOCALAPPDATA\Volta\bin;$env:Path"
  volta install node
}
if (-not (Get-Command npm -ErrorAction SilentlyContinue)) { volta install node }
if (-not (Get-Command netlify -ErrorAction SilentlyContinue)) { volta install netlify-cli }

# 2) Scaffold Next.js (+Tailwind)
npx --yes create-next-app@latest $AppName `
  --ts --eslint --app --tailwind --use-npm --yes

Set-Location $AppName

# 3) Build parity
(node -v).TrimStart('v') | Out-File -NoNewline .node-version -Encoding ascii
@'
[build]
  command = "npm run build"
'@ | Out-File -Encoding utf8 netlify.toml
"NEXT_PUBLIC_APP_NAME=$AppName" | Out-File -Encoding ascii .env

# 4) Start Netlify Dev locally, offline-capable
netlify dev --offline
```

---

## Tailwind CSS: defaults, v3 vs v4, and quality-of-life

* `create-next-app --tailwind` initializes Tailwind for you (config, PostCSS, globals). If you skip prompts with `--yes`, Tailwind is included automatically via the flag. ([Next.js][1])
* **If your project is on Tailwind v4** (check your `package.json`), global CSS uses the new import approach; for Better Auth UI, add:

  ```css
  /* app/globals.css (or styles/globals.css) */
  @import "@daveyplate/better-auth-ui/css";
  ```

  (**v3** users instead add the Better Auth UI content glob to `tailwind.config.*` as documented.) ([Better Auth UI][2])
* Optional: enable automatic Tailwind class sorting with Prettier:

  ```bash
  npm i -D prettier prettier-plugin-tailwindcss
  ```

  ```json
  // .prettierrc
  { "plugins": ["prettier-plugin-tailwindcss"] }
  ```

  This keeps class order consistent across the codebase. ([GitHub][3], [Tailwind CSS][4])
* Tailwind's official Next.js guide is a good reference for default file globs and setup. ([Tailwind CSS][5])

---

## Using the Netlify CLI **offline** (copy-ready)

* **Dev parity (hot reload + Netlify platform emulation, offline):**

  ```bash
  netlify dev --offline
  ```

  Optional:

  ```bash
  netlify dev --offline --context production
  ```

  ([Netlify CLI command reference][6])

* **Production parity (local build + serve, offline):**

  ```bash
  netlify build
  netlify serve --offline
  ```

  ([Netlify Docs][7])

* **Env vars locally (no account):** put values in a root `.env`. If you later link a site, `netlify dev --context production` will use remote envs; `--offline` uses local `.env`. ([Netlify Docs][7])

## Critical: Netlify Dev Port Configuration

**ALWAYS configure the `[dev]` block in `netlify.toml` to prevent port conflicts and proxy errors:**

```toml
[dev]
  targetPort = 3000  # Next.js development server port
  port = 8888        # Netlify Dev proxy server port  
  command = "npm run dev"
```

**Why this is essential:**
- Without explicit configuration, Netlify dev may fail to proxy correctly to Next.js
- Port conflicts with other running services cause connection errors
- Next.js may start on a different port (3001) if 3000 is occupied, breaking the proxy
- Explicit `targetPort` and `port` settings ensure reliable, predictable development environment

**Troubleshooting port conflicts:**
```bash
# Check what's running on common ports
lsof -i :3000 -i :8888

# Kill conflicting processes if needed
pkill -f "netlify\|next"

# Or kill specific PID
kill <PID>
```

**Never start development with just `npm run dev` when using Netlify functions/features - always use `netlify dev --offline` with proper port configuration.**

> **⚠️ Common pitfall: Port conflicts when starting the dev server**
> 
> When running `netlify dev --offline`, you may encounter port conflicts if other services are already running. To resolve:
> - Check what's running on common ports: `lsof -i :3000 -i :8888`
> - Kill conflicting processes: `pkill -f "netlify\|next"` 
> - Start with a specific port: `netlify dev --offline --port 8889`
> - Alternatively, kill the specific process by PID: `kill <PID>`

---

## Next.js + Netlify emulation **with Netlify Functions** (example components included)

**When to use what:**

* Use **Next.js Route Handlers** for app endpoints (`app/api/**`), compiled automatically into Netlify Functions.
* Add **standalone Netlify Functions** in `netlify/functions/**` when you want explicit serverless endpoints (cron/background, non‑Next code, special routing) or to keep secrets isolated.

**Project structure (excerpt):**

```
app/
  api/
    hello/route.ts            # Next Route Handler → auto Netlify Function
lib/
  env.ts
netlify/
  functions/secret.ts         # Standalone Netlify Function
netlify.toml
```

**Example 1 — Next.js Route Handler with SSR-friendly JSON:**

```ts
// app/api/hello/route.ts
export const runtime = 'nodejs'; // Ensure Node runtime for Node APIs if needed
export async function GET() {
  return Response.json({ ok: true, time: new Date().toISOString() });
}
```

(Use Node runtime when you need Node APIs; Edge does not support them.) ([Next.js][8])

**Example 2 — Standalone Netlify Function (reads secrets safely):**

```ts
// netlify/functions/secret.ts
import type { Handler } from '@netlify/functions';

export const handler: Handler = async () => {
  const apiKey = process.env.MY_BACKEND_API_KEY || 'not-set';
  return {
    statusCode: 200,
    body: JSON.stringify({ configured: apiKey !== 'not-set' })
  };
};
```

**Local testing (fully offline):**

```bash
netlify dev --offline
# Test:
#   http://localhost:8888/api/hello
#   http://localhost:8888/.netlify/functions/secret
```

---

## Next.js + Netlify emulation **with SQLite/libSQL** (example components included)

**Goal:** run offline using a local SQLite file (or `:memory:`), then switch to Turso in prod with no code changes.

**Install:**

```bash
npm i drizzle-orm @libsql/client
npm i -D drizzle-kit
```

**Drizzle config:**

```ts
// drizzle.config.ts
import { defineConfig } from 'drizzle-kit';

export default defineConfig({
  schema: './db/schema.ts',
  out: './drizzle',
  dialect: 'sqlite',
  dbCredentials: {
    url: process.env.DATABASE_URL || 'file:.data/dev.db'
  }
});
```

**Schema & DB wiring:**

```ts
// db/schema.ts
import { sqliteTable, text, integer } from 'drizzle-orm/sqlite-core';

export const todos = sqliteTable('todos', {
  id: integer('id').primaryKey({ autoIncrement: true }),
  title: text('title').notNull(),
  done: integer('done', { mode: 'boolean' }).notNull().default(false),
});
```

```ts
// lib/db.ts
import { drizzle } from 'drizzle-orm/libsql';
import { createClient } from '@libsql/client/node';

const url = process.env.TURSO_DATABASE_URL
  ? process.env.TURSO_DATABASE_URL
  : (process.env.DATABASE_URL || 'file:.data/dev.db');

const authToken = process.env.TURSO_DATABASE_URL ? process.env.TURSO_AUTH_TOKEN : undefined;

export const client = createClient({ url, authToken });
export const db = drizzle(client);
```

**Route Handler using the DB (Node runtime):**

```ts
// app/api/todos/route.ts
export const runtime = 'nodejs';

import { db } from '@/lib/db';
import { todos } from '@/db/schema';
import { eq } from 'drizzle-orm';

export async function GET() {
  const rows = await db.select().from(todos);
  return Response.json(rows);
}

export async function POST(req: Request) {
  const body = await req.json();
  await db.insert(todos).values({ title: String(body.title ?? ''), done: !!body.done });
  return Response.json({ ok: true });
}

export async function DELETE(req: Request) {
  const id = Number(new URL(req.url).searchParams.get('id'));
  if (Number.isFinite(id)) {
    await db.delete(todos).where(eq(todos.id, id));
  }
  return Response.json({ ok: true });
}
```

**Environment:**

```
# .env (local)
DATABASE_URL=file:.data/dev.db
BETTER_AUTH_SECRET=dev-secret-please-change
```

**Important runtime tip:** any DB access that touches a file path must run with `export const runtime = 'nodejs'` (Edge runtime does not expose Node APIs). ([Next.js][8])

> **⚠️ Common pitfall: Drizzle push command fails**
> 
> When running `npx drizzle-kit push`, you may get an error about missing environment variables. This happens because:
> - The push command expects `TURSO_DATABASE_URL` and `TURSO_AUTH_TOKEN` for production
> - For local development, create the tables manually using a migration script instead
> - Use `npx drizzle-kit generate` to create migration files, then apply them with a custom script that reads from your local `.env`

---

## **Better Auth** on Netlify (Next.js App Router + Better Auth UI)

**Install packages (server + client UI):**

```bash
npm i better-auth @daveyplate/better-auth-ui
```

**Tailwind styling for Better Auth UI:**

* **Tailwind v4**: add to your global CSS:

  ```css
  @import "@daveyplate/better-auth-ui/css";
  ```
* **Tailwind v3**: add the Better Auth UI content glob to your `tailwind.config.*`:

  ```ts
  content: [
    "./node_modules/@daveyplate/better-auth-ui/dist/**/*.{js,ts,jsx,tsx,mdx}"
  ]
  ```

(These are the officially documented Tailwind v3/v4 paths for Better Auth UI.) ([Better Auth UI][2])

> **⚠️ Common pitfall: Poor contrast in Better Auth UI**
> 
> Better Auth UI may have poor contrast with light text on light backgrounds. To fix:
> - Add custom CSS overrides to improve readability:
> ```css
> /* Better Auth UI Custom Styles for Better Contrast */
> .better-auth-ui input,
> .better-auth-ui input[type="email"],
> .better-auth-ui input[type="password"],
> .better-auth-ui input[type="text"] {
>   color: #1f2937 !important;
>   background-color: #ffffff !important;
>   border: 1px solid #d1d5db !important;
> }
> 
> .better-auth-ui input:focus {
>   border-color: #f59e0b !important;
>   box-shadow: 0 0 0 3px rgba(245, 158, 11, 0.1) !important;
> }
> 
> .better-auth-ui label {
>   color: #374151 !important;
>   font-weight: 500 !important;
> }
> ```
> - Wrap auth components in a container with the `better-auth-ui` class to apply styles

**Server: create the Better Auth instance using Drizzle + libSQL**

```ts
// lib/auth.ts
import { betterAuth } from 'better-auth';
import { drizzleAdapter } from 'better-auth/adapters/drizzle';
import { db } from '@/lib/db';

export const auth = betterAuth({
  secret: process.env.BETTER_AUTH_SECRET!,
  database: drizzleAdapter(db, { provider: 'sqlite' }),
  emailAndPassword: { enabled: true },
});
```

> **⚠️ Common pitfall: Better Auth table schema issues**
> 
> Better Auth needs to auto-generate its own tables (`user`, `session`, `account`, `verification`). If you get errors like "The model 'user' was not found in the schema object":
> - Define the Better Auth tables in your Drizzle schema manually:
> ```ts
> export const user = sqliteTable('user', {
>   id: text('id').primaryKey(),
>   email: text('email').notNull().unique(),
>   emailVerified: integer('emailVerified', { mode: 'boolean' }).notNull().default(false),
>   name: text('name'),
>   image: text('image'),
>   createdAt: integer('createdAt', { mode: 'timestamp' }).notNull().$default(() => new Date()),
>   updatedAt: integer('updatedAt', { mode: 'timestamp' }).notNull().$default(() => new Date()),
> });
> // ... plus session, account, verification tables
> ```
> - Pass the schema to the adapter:
> ```ts
> database: drizzleAdapter(db, { 
>   provider: 'sqlite',
>   schema: { user, session, account, verification }
> })
> ```
> - Run migrations to create the tables before testing auth functionality

**Route handler mount (App Router):**

```ts
// app/api/auth/[...all]/route.ts
import { auth } from '@/lib/auth';
import { toNextJsHandler } from 'better-auth/next-js';
export const { GET, POST } = toNextJsHandler(auth.handler);
```

**Client: Better Auth UI provider + client**

```ts
// lib/auth-client.ts
import { createAuthClient } from 'better-auth/react';
export const authClient = createAuthClient({});
```

```tsx
// app/providers.tsx
'use client';
import { AuthUIProvider } from '@daveyplate/better-auth-ui';
import Link from 'next/link';
import { useRouter } from 'next/navigation';
import type { ReactNode } from 'react';
import { authClient } from '@/lib/auth-client';

export function Providers({ children }: { children: ReactNode }) {
  const router = useRouter();
  return (
    <AuthUIProvider
      authClient={authClient}
      navigate={router.push}
      replace={router.replace}
      onSessionChange={() => router.refresh()}
      Link={Link}
    >
      {children}
    </AuthUIProvider>
  );
}
```

(See Better Auth UI's App Router guide for these exact conventions and the prebuilt pages/components.) ([Better Auth UI][9])

**Auth pages (Tailwind‑styled out of the box):**

```tsx
// app/auth/[authView]/page.tsx
import { AuthView } from '@daveyplate/better-auth-ui';
import { authViewPaths } from '@daveyplate/better-auth-ui/server';

export const dynamicParams = false;
export function generateStaticParams() {
  return Object.values(authViewPaths).map((authView) => ({ authView }));
}
export default async function AuthPage({ params }: { params: Promise<{ authView: string }> }) {
  const { authView } = await params;
  return <AuthView pathname={authView} />;
}
```

(Better Auth UI ships Tailwind‑styled components and works seamlessly with Tailwind.) ([Better Auth UI][10])

---

# Deployment considerations

## Deploying to **Netlify** (example commands)

```bash
# (one-time) authenticate locally, or use NETLIFY_AUTH_TOKEN in CI
netlify login

# link local folder to a site (or use `netlify init` to create one)
netlify link

# import env vars from your local .env (site-scoped)
netlify env:import .env

# build locally (optional parity check)
netlify build

# deploy to a draft URL (Preview)
netlify deploy --build

# deploy to production
netlify deploy --build --prod
```

(See CLI command reference for `dev`, `build`, `serve`, and `deploy`, plus `NETLIFY_AUTH_TOKEN`/`NETLIFY_SITE_ID` overrides.) ([Netlify CLI command reference][11])

## Deploying to **Turso.tech** (example commands)

```bash
# install + auth Turso CLI
# macOS/Linux: curl -sSf https://get.turso.tech | bash
# Windows: scoop bucket add tursodb https://github.com/tursodatabase/scoop-bucket && scoop install turso
turso auth signup   # or: turso auth login

# create a database
turso db create myapp

# get the database URL and create a long-lived token
turso db show myapp --url
turso db tokens create myapp -e none

# set these on the linked Netlify site
netlify env:set TURSO_DATABASE_URL "libsql://<org>-myapp.turso.io"
netlify env:set TURSO_AUTH_TOKEN "<token>"
```

**Drizzle schema init (prod):**

* Run your migration step in CI or as a one‑off script using the same `@libsql/client` your app uses.

## Configuring Netlify and Turso to work together

* Store `TURSO_DATABASE_URL` and `TURSO_AUTH_TOKEN` as Netlify env vars (production and preview contexts as needed).
* Use the libSQL client in **Node** runtime code paths; keep Edge Middleware light (avoid DB/file access).
* For local dev, use a file DB (`file:.data/dev.db`) or `:memory:`; switching to Turso requires only environment changes.
* Netlify's Next.js adapter handles builds automatically; you do not need extra config for Tailwind. ([Netlify Docs][12])

---

## Common Pitfalls & Quick Fixes

### Better Auth Integration
- **Hook imports**: Use `authClient.getSession()` directly in components rather than importing hooks from `@daveyplate/better-auth-ui` that may not exist. The auth client pattern is more reliable.
- **Table schema**: Always define Better Auth tables (user, session, account, verification) explicitly in your Drizzle schema and pass them to the adapter to avoid "model not found" errors:
  ```ts
  database: drizzleAdapter(db, { 
    provider: 'sqlite',
    schema: { user, session, account, verification }
  })
  ```
- **CSS imports**: Add Better Auth UI styles to globals.css and include custom contrast improvements for better readability.

### Drizzle Configuration
- **Database URL**: Always include `dbCredentials` in drizzle.config.ts:
  ```ts
  export default defineConfig({
    schema: './db/schema.ts',
    dialect: 'sqlite',
    dbCredentials: {
      url: process.env.DATABASE_URL || 'file:.data/dev.db'
    }
  });
  ```
- **Push before seed**: Run `npm run db:push` before `npm run db:seed` to ensure tables exist.

### Runtime & API Routes
- **Node runtime**: Any API route using SQLite files, filesystem, or Node APIs must specify `export const runtime = 'nodejs'` at the top.
- **Session handling**: Use Better Auth's built-in session checking in protected API routes.

### Development Workflow
- **Package installation**: Allow time for npm installations to complete, especially for large dependency sets.
- **Port conflicts**: Check for and kill conflicting processes on ports 3000 and 8888 before starting `netlify dev`.
- **Database initialization**: Create `.data` directory, run schema push, then seed in that order.
