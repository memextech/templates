# Brutalist Functional

### Overview
A raw, honest design system that prioritizes function over form, embracing the inherent properties of web technologies. Perfect for developer tools, technical documentation, and applications where authenticity and efficiency matter more than visual polish.

### Typography Specifications

**Primary Font Strategy**: Use system fonts exclusively for maximum performance and authenticity.

**System Font Stack**:
- Primary: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, Cantarell, sans-serif
- Monospace: 'SF Mono', Monaco, 'Cascadia Code', 'Roboto Mono', Consolas, 'Courier New', monospace

**Heading Typography**:
- H1: System font Bold (700 weight) at 48px (3rem) with normal letter spacing
- H2: System font Bold (700 weight) at 36px (2.25rem) with normal letter spacing
- H3: System font Bold (700 weight) at 28px (1.75rem) with normal letter spacing
- H4: System font Bold (700 weight) at 22px (1.375rem) with normal letter spacing
- H5: System font Bold (700 weight) at 18px (1.125rem) with normal letter spacing

**Body Typography**:
- Primary body text: System font Regular (400 weight) at 16px (1rem) with 1.5 line height
- Secondary body text: System font Regular (400 weight) at 14px (0.875rem) with 1.4 line height
- Small text: System font Regular (400 weight) at 12px (0.75rem) with 1.3 line height

**Technical Typography**:
- Code blocks: Monospace font Regular (400 weight) at 14px with 1.4 line height
- Inline code: Monospace font Regular (400 weight) at 13px with background highlight
- Data tables: Monospace font Regular (400 weight) at 13px for precise alignment

### Color Palette Specifications

**Monochromatic Foundation**:
- Primary background: #ffffff (pure white, no compromise)
- Secondary background: #f5f5f5 (neutral-100 equivalent, minimal differentiation)
- Tertiary background: #e5e5e5 (neutral-200 equivalent, for input fields)
- Border color: #000000 (pure black, maximum contrast)
- Subtle border: #cccccc (neutral-300 equivalent, for internal divisions)

**High Contrast Text**:
- Primary text: #000000 (pure black for maximum readability)
- Secondary text: #333333 (dark gray for hierarchy)
- Tertiary text: #666666 (medium gray for metadata)
- Disabled text: #999999 (light gray for inactive elements)
- Inverted text: #ffffff (white for dark backgrounds)

**Functional Accent Colors**:
- Primary action: #0000ff (pure blue, web-native color)
- Secondary action: #000000 (black for secondary buttons)
- Success: #008000 (pure green, universally understood)
- Warning: #ffa500 (orange, attention-grabbing)
- Error: #ff0000 (pure red, immediate recognition)
- Info: #0000ff (blue, consistent with primary action)

**System Colors**:
- Link color: #0000ff (classic web blue)
- Visited link: #800080 (purple, browser default)
- Focus outline: #0000ff (matches link color)
- Selection background: #b3d4fc (browser default blue)

### Layout and Spacing

**Rigid Grid System**:
Use a strict 12-column grid with 20px gutters. No responsive grid changes - maintain consistent structure across all screen sizes for predictable behavior.

**Systematic Spacing (4px base unit)**:
- xs: 4px (0.25rem) - minimal spacing for tight layouts
- sm: 8px (0.5rem) - small component spacing
- md: 16px (1rem) - standard spacing unit
- lg: 32px (2rem) - section spacing
- xl: 64px (4rem) - major section breaks

**Brutalist Layout Principles**:
- No rounded corners (border-radius: 0 for all elements)
- Sharp, defined edges with solid borders
- Generous whitespace for content breathing room
- Asymmetrical layouts that follow content hierarchy
- No decorative elements or visual flourishes

**Container Specifications**:
- Page margins: 20px on all screen sizes
- Content max-width: 1200px with no centering (left-aligned)
- Section spacing: 64px vertical between major sections
- Component spacing: 32px between related components

### Animation and Interaction

**Minimal Animation Philosophy**:
- No decorative animations or transitions
- Instant state changes for immediate feedback
- Focus on functional animations only (loading, progress)
- Respect user preferences for reduced motion

**Functional Interactions**:
- Hover states: Add 2px solid black border, no transition
- Focus states: 3px solid blue outline, no transition
- Active states: Invert colors (black background, white text)
- Loading states: Simple text indicators ("Loading...") or basic progress bars

**Performance-First Approach**:
- No CSS transitions or animations unless functionally necessary
- Instant visual feedback for all user interactions
- Minimal DOM manipulation for state changes
- No JavaScript animations - CSS only when required

### Component Specifications

**Brutalist Navigation**:
Design navigation as a simple horizontal list with black text on white background. Current page should have black background with white text. No hover effects except focus outline. Include breadcrumbs as plain text with " > " separators.

**Raw Form Elements**:
Create form elements using browser defaults with minimal styling. Input fields should have 1px solid black border, no border-radius, and 8px padding. Labels should be positioned above inputs with bold font weight. Required fields marked with plain asterisk.

**Functional Buttons**:
Design buttons with black 2px solid border, white background, black text, and 12px vertical, 16px horizontal padding. Primary buttons should have black background with white text. No hover effects except focus outline.

**Plain Data Tables**:
Create tables with 1px solid black borders on all cells. Header cells should have black background with white text. Alternate row backgrounds using secondary background color. No sorting indicators unless functionally necessary.

**Honest Cards**:
Design cards as simple containers with 2px solid black border and white background. Include 20px padding on all sides. No shadows, rounded corners, or decorative elements. Content should be left-aligned with clear hierarchy.

**System Alerts**:
Create alert components using appropriate functional colors with 2px solid borders. Include bold text for alert type and regular text for message. No icons unless they serve a functional purpose.

### Accessibility Specifications

**Maximum Contrast**:
Ensure all text uses pure black (#000000) on white (#ffffff) or pure white on black for maximum possible contrast ratio (21:1). No gray text except for clearly hierarchical purposes.

**Keyboard-First Design**:
Design all interactions to work perfectly with keyboard navigation. Focus indicators should be highly visible (3px solid blue outline). Tab order should follow logical content flow.

**Screen Reader Optimization**:
Use semantic HTML elements exclusively. Avoid div/span soup in favor of proper headings, lists, tables, and form elements. Include descriptive text for all functional elements.

**No Reliance on Color**:
Ensure all information conveyed by color is also available through text, shape, or position. Use text labels alongside color coding for status indicators.

### Responsive Behavior

**Content-First Responsive**:
Design responsive behavior based on content needs rather than device assumptions. Use min-width media queries to progressively enhance layouts.

**Consistent Experience**:
Maintain the same functionality and visual hierarchy across all screen sizes. No hidden navigation or collapsed content - everything should be accessible at all times.

**Performance Responsive**:
Optimize for performance across all devices by avoiding complex layouts, animations, or heavy visual effects. Fast loading and interaction response times are prioritized over visual appeal.

**Touch Accessibility**:
Ensure touch targets are minimum 44px with adequate spacing. Maintain the brutalist aesthetic while meeting accessibility requirements for mobile users.

### Technical Implementation

**Minimal CSS**:
Write CSS that embraces browser defaults and web standards. Avoid CSS frameworks or preprocessors. Use simple, declarative styles that are easy to understand and maintain.

**Semantic HTML**:
Use proper HTML5 semantic elements (header, nav, main, section, article, aside, footer) for document structure. Avoid unnecessary div elements and CSS classes.

**Progressive Enhancement**:
Build core functionality with HTML and CSS first, then enhance with minimal JavaScript only when necessary. Ensure the interface works without JavaScript enabled.

**Web Standards Compliance**:
Follow web standards strictly. Use valid HTML5, standard CSS properties, and avoid vendor prefixes unless absolutely necessary for functionality. 