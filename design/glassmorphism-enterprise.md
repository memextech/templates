# Glassmorphism Enterprise

### Overview
A premium glassmorphism design system that creates depth and sophistication through translucent elements and backdrop blur effects. Perfect for modern business applications, product showcases, and pre-sales demonstrations that need to convey innovation and premium quality.

### Typography Specifications

**Primary Font Pairing**: Use Bricolage Grotesque for headings and Inter for body text to create contemporary contrast.

**Heading Typography**:
- H1: Bricolage Grotesque Bold (700 weight) at 56px (3.5rem) with -0.03em letter spacing
- H2: Bricolage Grotesque SemiBold (600 weight) at 40px (2.5rem) with -0.02em letter spacing
- H3: Bricolage Grotesque Medium (500 weight) at 28px (1.75rem) with -0.01em letter spacing
- H4: Bricolage Grotesque Medium (500 weight) at 22px (1.375rem) with normal letter spacing

**Body Typography**:
- Primary body text: Inter Regular (400 weight) at 17px (1.0625rem) with 1.65 line height
- Secondary body text: Inter Regular (400 weight) at 15px (0.9375rem) with 1.6 line height
- Caption text: Inter Medium (500 weight) at 13px (0.8125rem) with 1.5 line height

**Special Typography**:
- Feature callouts: Bricolage Grotesque SemiBold (600 weight) at 20px with 1.4 line height
- Button text: Inter SemiBold (600 weight) at 16px with 0.01em letter spacing

### Color Palette Specifications

**Background System**:
- Primary background: Linear gradient from #0f172a to #1e293b (slate-900 to slate-800)
- Secondary background: Linear gradient from #1e293b to #334155 (slate-800 to slate-600)
- Glass surface base: rgba(255, 255, 255, 0.05) with backdrop-filter: blur(20px)
- Glass surface elevated: rgba(255, 255, 255, 0.08) with backdrop-filter: blur(24px)

**Text Colors**:
- Primary text: #f8fafc (slate-50, high contrast on glass surfaces)
- Secondary text: #cbd5e1 (slate-300, for descriptions and secondary information)
- Tertiary text: #94a3b8 (slate-400, for captions and metadata)
- Glass text overlay: rgba(248, 250, 252, 0.95) for text over complex backgrounds

**Accent Colors**:
- Primary accent: Linear gradient from #3b82f6 to #6366f1 (blue-500 to indigo-500)
- Primary accent hover: Linear gradient from #2563eb to #4f46e5 (blue-600 to indigo-600)
- Secondary accent: Linear gradient from #8b5cf6 to #a855f7 (purple-500 to purple-500)
- Success: #10b981 with 20% opacity glass overlay
- Warning: #f59e0b with 20% opacity glass overlay
- Error: #ef4444 with 20% opacity glass overlay

**Glass Border System**:
- Primary glass border: 1px solid rgba(255, 255, 255, 0.1)
- Secondary glass border: 1px solid rgba(255, 255, 255, 0.05)
- Accent glass border: 1px solid rgba(59, 130, 246, 0.3)

### Glassmorphism Effects Specifications

**Primary Glass Cards**:
Create cards with background: rgba(255, 255, 255, 0.05), backdrop-filter: blur(20px), border: 1px solid rgba(255, 255, 255, 0.1), border-radius: 16px, and box-shadow: 0 8px 32px rgba(0, 0, 0, 0.1).

**Elevated Glass Elements**:
For navigation bars and modal overlays, use background: rgba(255, 255, 255, 0.08), backdrop-filter: blur(24px), border: 1px solid rgba(255, 255, 255, 0.15), and box-shadow: 0 12px 40px rgba(0, 0, 0, 0.15).

**Interactive Glass States**:
On hover, transition glass elements to background: rgba(255, 255, 255, 0.08) and border: 1px solid rgba(255, 255, 255, 0.2) over 250ms ease-out. Add a subtle glow effect with box-shadow: 0 8px 32px rgba(59, 130, 246, 0.1).

**Glass Button Effects**:
Primary buttons should have background: linear-gradient(135deg, rgba(59, 130, 246, 0.8), rgba(99, 102, 241, 0.8)), backdrop-filter: blur(16px), border: 1px solid rgba(255, 255, 255, 0.2), and border-radius: 12px.

### Layout and Spacing

**Grid System with Glass Hierarchy**:
Implement a 12-column grid with glass containers that create visual depth. Primary content areas should use elevated glass effects, while secondary content uses primary glass effects.

**Spacing for Glass Elements**:
- Glass card padding: 32px on all sides for desktop, 24px for tablet, 16px for mobile
- Glass element margins: 24px between glass cards, 16px between glass components
- Glass border radius: 16px for large elements, 12px for medium elements, 8px for small elements

**Layered Layout System**:
Create visual depth by layering glass elements with different opacity levels. Background elements should use 0.03 opacity, mid-ground elements 0.05 opacity, and foreground elements 0.08 opacity.

### Animation and Interaction

**Glass Transition Timing**:
- Glass opacity changes: 300ms ease-in-out
- Backdrop blur transitions: 400ms ease-out
- Glass element scaling: 250ms ease-out
- Border glow effects: 200ms ease-in-out

**Glass Card Animations**:
Implement hover effects that increase backdrop blur from 20px to 28px, scale to 1.02x, and add a subtle glow with box-shadow: 0 12px 40px rgba(59, 130, 246, 0.15) over 300ms ease-out.

**Glass Button Interactions**:
Create button press effects that briefly reduce opacity to 0.8 and scale to 0.98x for 100ms, then return to normal state. Include a ripple effect that expands from click point with rgba(255, 255, 255, 0.3) color.

**Loading Glass Effects**:
For loading states, create a shimmer effect that moves across glass surfaces using a linear gradient animation: linear-gradient(90deg, transparent, rgba(255, 255, 255, 0.1), transparent) moving from left to right over 1.5 seconds.

**Text Reveal Animations**:
Implement text animations where headings fade in with a blur-to-focus effect, starting with filter: blur(8px) and opacity: 0, transitioning to filter: blur(0px) and opacity: 1 over 600ms ease-out with 150ms stagger between lines.

### Component Specifications

**Glass Navigation**:
Design a navigation bar with background: rgba(255, 255, 255, 0.08), backdrop-filter: blur(24px), height: 72px, and border-bottom: 1px solid rgba(255, 255, 255, 0.1). Navigation items should have 16px padding and transition to rgba(255, 255, 255, 0.1) background on hover.

**Glass Modal System**:
Create modals with background: rgba(255, 255, 255, 0.1), backdrop-filter: blur(32px), border-radius: 20px, and box-shadow: 0 20px 60px rgba(0, 0, 0, 0.2). Modal backdrop should be rgba(0, 0, 0, 0.4) with backdrop-filter: blur(8px).

**Glass Form Elements**:
Design input fields with background: rgba(255, 255, 255, 0.05), backdrop-filter: blur(16px), border: 1px solid rgba(255, 255, 255, 0.1), border-radius: 12px, and 16px padding. Focus state should add border: 1px solid rgba(59, 130, 246, 0.5) and box-shadow: 0 0 0 3px rgba(59, 130, 246, 0.1).

**Glass Data Visualization**:
For charts and graphs, use glass containers with background: rgba(255, 255, 255, 0.03) and subtle grid lines with rgba(255, 255, 255, 0.1). Data points should have glass-like appearances with backdrop blur effects.

### Responsive Behavior

**Glass Effect Scaling**:
On mobile devices, reduce backdrop blur values by 25% for performance optimization. Primary glass elements should use blur(15px) instead of blur(20px), and elevated elements should use blur(18px) instead of blur(24px).

**Adaptive Glass Opacity**:
Increase glass opacity by 20% on smaller screens for better content visibility. Mobile glass cards should use rgba(255, 255, 255, 0.06) instead of rgba(255, 255, 255, 0.05).

**Touch-Optimized Glass Interactions**:
Increase touch targets to minimum 48px height with 8px margin between interactive glass elements. Implement touch feedback with brief opacity reduction and subtle scale animation.

**Performance Considerations**:
On devices with limited processing power, provide fallback styles without backdrop-filter, using solid colors with appropriate opacity levels to maintain visual hierarchy. 