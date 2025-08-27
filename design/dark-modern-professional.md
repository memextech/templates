# Dark Modern Professional

### Overview
A sophisticated dark theme optimized for professional applications, dashboards, and business tools. This theme reduces eye strain during extended use while conveying technical competence and modern sensibilities.

### Typography Specifications

**Primary Font Pairing**: Use Inter for all text elements with specific weight variations for hierarchy.

**Heading Typography**:
- H1: Inter Bold (700 weight) at 48px (3rem) with -0.02em letter spacing
- H2: Inter SemiBold (600 weight) at 36px (2.25rem) with -0.01em letter spacing  
- H3: Inter Medium (500 weight) at 24px (1.5rem) with normal letter spacing
- H4: Inter Medium (500 weight) at 20px (1.25rem) with normal letter spacing

**Body Typography**:
- Primary body text: Inter Regular (400 weight) at 16px (1rem) with 1.6 line height
- Secondary body text: Inter Regular (400 weight) at 14px (0.875rem) with 1.5 line height
- Caption text: Inter Regular (400 weight) at 12px (0.75rem) with 1.4 line height

**Code Typography**:
- Code blocks and technical content: JetBrains Mono Regular (400 weight) at 14px with 1.5 line height
- Inline code: JetBrains Mono Regular (400 weight) at 13px with subtle background highlight

### Color Palette Specifications

**Background Colors**:
- Primary background: #0a0a0a (near-black for reduced eye strain)
- Secondary background: #141414 (cards, panels, elevated surfaces)
- Tertiary background: #1f1f1f (input fields, code blocks)
- Overlay background: rgba(10, 10, 10, 0.95) for modals and dropdowns

**Text Colors**:
- Primary text: #ffffff (high contrast for readability)
- Secondary text: #a3a3a3 (neutral-400 equivalent, for descriptions and labels)
- Tertiary text: #737373 (neutral-500 equivalent, for captions and metadata)
- Disabled text: #525252 (neutral-600 equivalent)

**Accent Colors**:
- Primary accent: #3b82f6 (blue-500, for CTAs and primary actions)
- Primary accent hover: #2563eb (blue-600)
- Secondary accent: #8b5cf6 (purple-500, for secondary actions)
- Success: #10b981 (emerald-500)
- Warning: #f59e0b (amber-500)
- Error: #ef4444 (red-500)

**Border Colors**:
- Primary borders: #262626 (neutral-800 equivalent)
- Secondary borders: #171717 (neutral-900 equivalent)
- Focus borders: #3b82f6 (matches primary accent)

### Layout and Spacing

**Grid System**:
Create layouts using a 12-column grid with 24px gutters on desktop (1024px+), 16px gutters on tablet (768px-1023px), and 12px gutters on mobile (below 768px).

**Spacing Scale**:
- xs: 4px (0.25rem) - for tight spacing between related elements
- sm: 8px (0.5rem) - for small gaps and padding
- md: 16px (1rem) - for standard spacing between components
- lg: 24px (1.5rem) - for section spacing
- xl: 32px (2rem) - for major section breaks
- 2xl: 48px (3rem) - for page-level spacing

**Component Spacing**:
- Card padding: 24px on all sides for desktop, 16px for mobile
- Button padding: 12px vertical, 24px horizontal for primary buttons
- Input padding: 12px vertical, 16px horizontal
- Navigation spacing: 16px between navigation items

### Animation and Interaction

**Transition Timing**:
- Micro-interactions (hover states): 150ms ease-out
- Component state changes: 250ms ease-in-out
- Page transitions: 300ms ease-in-out
- Loading animations: 400ms ease-in-out

**Button Animations**:
Create button hover effects that scale to 1.02x size and shift background color from primary accent to primary accent hover with a 150ms ease-out transition. Include a subtle shadow that increases from 0 4px 6px rgba(0, 0, 0, 0.1) to 0 8px 15px rgba(0, 0, 0, 0.2).

**Card Animations**:
Implement card hover effects that lift the element with a shadow transition from 0 1px 3px rgba(0, 0, 0, 0.1) to 0 10px 25px rgba(0, 0, 0, 0.2) over 250ms ease-out. Add a subtle 1.01x scale transform.

**Loading States**:
Use a pulsing animation for skeleton loaders with opacity transitioning between 0.4 and 0.8 over 1.5 seconds in an infinite ease-in-out loop.

**Text Animations**:
For page headings, implement a fade-up animation where text starts 20px below final position with 0 opacity, then animates to final position with full opacity over 400ms ease-out with a 100ms stagger between words.

### Component Specifications

**Navigation Bar**:
Design a sticky navigation bar with 64px height, primary background color, and 1px bottom border using primary border color. Logo should be left-aligned with 24px left margin. Navigation links should be center-aligned using secondary text color, changing to primary text color on hover with a 150ms transition. User actions (login, profile) should be right-aligned with 24px right margin.

**Cards and Panels**:
Create cards with secondary background color, 8px border radius, and subtle border using primary border color. Include 24px padding on all sides. Add a subtle shadow: 0 1px 3px rgba(0, 0, 0, 0.1). For interactive cards, implement the hover animation specified above.

**Form Elements**:
Design input fields with tertiary background color, 6px border radius, and 1px border using primary border color. On focus, change border color to primary accent and add a 0 0 0 3px rgba(59, 130, 246, 0.1) box shadow. Placeholder text should use tertiary text color.

**Buttons**:
Primary buttons should use primary accent background with white text, 8px border radius, and the animation specifications above. Secondary buttons should have transparent background with primary accent border and primary accent text color. Disabled buttons should use neutral-600 background with neutral-400 text.

**Data Tables**:
Create tables with alternating row backgrounds using primary and secondary background colors. Header rows should use tertiary background with medium font weight. Include 12px vertical padding and 16px horizontal padding for cells. Add subtle borders between columns using primary border color.

### Responsive Behavior

**Breakpoint Strategy**:
- Mobile: 320px-767px (single column layouts, stacked navigation)
- Tablet: 768px-1023px (2-column layouts, condensed navigation)  
- Desktop: 1024px+ (multi-column layouts, full navigation)

**Navigation Transformation**:
On desktop, display full horizontal navigation. On tablet, condense navigation items and reduce spacing. On mobile, implement a hamburger menu that slides in from the left with a 300ms ease-in-out transition, revealing navigation items with a staggered fade-in animation (50ms delay between items).

**Typography Scaling**:
Reduce heading sizes by 20% on tablet and 30% on mobile while maintaining proportional relationships. Increase line height to 1.7 for body text on mobile for improved readability.

**Component Adaptation**:
Cards should maintain 24px padding on desktop, reduce to 16px on tablet, and 12px on mobile. Button padding should reduce proportionally. Tables should become horizontally scrollable on mobile with sticky first columns. 