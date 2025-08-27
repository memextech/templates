# Minimalist B2B Professional

### Overview
A clean, trustworthy design system optimized for business applications, enterprise software, and professional services. This theme prioritizes clarity, accessibility, and user confidence through purposeful simplicity and proven design patterns.

### Typography Specifications

**Primary Font Pairing**: Use Inter exclusively for maximum readability and professional appearance across all text elements.

**Heading Typography**:
- H1: Inter Bold (700 weight) at 42px (2.625rem) with -0.025em letter spacing
- H2: Inter SemiBold (600 weight) at 32px (2rem) with -0.02em letter spacing
- H3: Inter SemiBold (600 weight) at 24px (1.5rem) with -0.015em letter spacing
- H4: Inter Medium (500 weight) at 20px (1.25rem) with -0.01em letter spacing
- H5: Inter Medium (500 weight) at 18px (1.125rem) with normal letter spacing

**Body Typography**:
- Primary body text: Inter Regular (400 weight) at 16px (1rem) with 1.6 line height
- Secondary body text: Inter Regular (400 weight) at 14px (0.875rem) with 1.55 line height
- Small text: Inter Regular (400 weight) at 12px (0.75rem) with 1.5 line height
- Legal/fine print: Inter Regular (400 weight) at 11px (0.6875rem) with 1.45 line height

**Interface Typography**:
- Button text: Inter SemiBold (600 weight) at 15px with 0.005em letter spacing
- Navigation text: Inter Medium (500 weight) at 15px with normal letter spacing
- Form labels: Inter Medium (500 weight) at 14px with normal letter spacing
- Table headers: Inter SemiBold (600 weight) at 13px with 0.01em letter spacing

### Color Palette Specifications

**Neutral Foundation**:
- Primary background: #ffffff (pure white for maximum clarity)
- Secondary background: #f8fafc (slate-50, for subtle section differentiation)
- Tertiary background: #f1f5f9 (slate-100, for input fields and disabled states)
- Border color: #e2e8f0 (slate-200, for subtle divisions)
- Strong border: #cbd5e1 (slate-300, for emphasized divisions)

**Text Hierarchy**:
- Primary text: #0f172a (slate-900, maximum contrast for readability)
- Secondary text: #475569 (slate-600, for descriptions and secondary information)
- Tertiary text: #64748b (slate-500, for captions and metadata)
- Disabled text: #94a3b8 (slate-400, for inactive elements)
- Placeholder text: #cbd5e1 (slate-300, for form placeholders)

**Professional Accent System**:
- Primary accent: #2563eb (blue-600, trustworthy and professional)
- Primary accent hover: #1d4ed8 (blue-700)
- Primary accent light: #dbeafe (blue-100, for backgrounds and highlights)
- Secondary accent: #059669 (emerald-600, for success and positive actions)
- Warning accent: #d97706 (amber-600, for cautions and warnings)
- Error accent: #dc2626 (red-600, for errors and destructive actions)

**Status Colors**:
- Success background: #dcfce7 (green-100)
- Success border: #bbf7d0 (green-200)
- Warning background: #fef3c7 (amber-100)
- Warning border: #fde68a (amber-200)
- Error background: #fee2e2 (red-100)
- Error border: #fecaca (red-200)
- Info background: #dbeafe (blue-100)
- Info border: #bfdbfe (blue-200)

### Layout and Spacing

**Grid System**:
Implement a 12-column grid with 24px gutters on desktop (1200px+), 20px gutters on tablet (768px-1199px), and 16px gutters on mobile (below 768px). Use max-width of 1200px for main content areas with auto margins for centering.

**Spacing Scale (8px base unit)**:
- 2xs: 2px (0.125rem) - for fine adjustments and borders
- xs: 4px (0.25rem) - for tight spacing between related elements
- sm: 8px (0.5rem) - for small gaps and compact padding
- md: 16px (1rem) - for standard component spacing
- lg: 24px (1.5rem) - for section spacing and card padding
- xl: 32px (2rem) - for major section breaks
- 2xl: 48px (3rem) - for page-level spacing and hero sections
- 3xl: 64px (4rem) - for large section breaks

**Component Spacing Standards**:
- Page margins: 24px on desktop, 16px on mobile
- Card padding: 24px on all sides
- Form element spacing: 16px vertical between fields
- Button padding: 12px vertical, 20px horizontal for primary buttons
- Table cell padding: 12px vertical, 16px horizontal
- Navigation item spacing: 8px between items, 16px padding within items

### Animation and Interaction

**Transition Timing (Performance Optimized)**:
- Micro-interactions: 150ms ease-out (hover states, focus indicators)
- Component transitions: 200ms ease-in-out (state changes, reveals)
- Page transitions: 250ms ease-in-out (navigation, modal appearances)
- Loading states: 300ms ease-in-out (skeleton screens, progress indicators)

**Hover and Focus States**:
Create subtle hover effects that change background color from transparent to slate-50 over 150ms ease-out. Focus states should add a 2px solid primary accent border and 0 0 0 3px rgba(37, 99, 235, 0.1) box shadow.

**Button Interactions**:
Primary buttons should darken from primary accent to primary accent hover over 150ms ease-out. Include a subtle shadow that increases from 0 1px 2px rgba(0, 0, 0, 0.05) to 0 2px 4px rgba(0, 0, 0, 0.1). Secondary buttons should add slate-100 background on hover.

**Form Feedback**:
Input fields should transition border color from border color to primary accent on focus over 150ms ease-out. Error states should transition border to error accent with a subtle shake animation (3px left-right movement over 200ms).

**Loading Animations**:
Use a subtle pulse animation for skeleton loaders with opacity transitioning between 0.6 and 1.0 over 1.2 seconds in an infinite ease-in-out loop. Progress bars should have a smooth indeterminate animation moving from left to right over 2 seconds.

### Component Specifications

**Navigation System**:
Design a clean navigation bar with 64px height, white background, and 1px bottom border using border color. Logo should be left-aligned with Inter SemiBold at 20px. Navigation links should use navigation text specifications with 8px horizontal padding and 32px vertical padding. Active states should have primary accent text color with a 2px bottom border.

**Card Components**:
Create cards with white background, 1px border using border color, 8px border radius, and subtle shadow: 0 1px 3px rgba(0, 0, 0, 0.1). Include 24px padding on all sides. Interactive cards should add hover shadow: 0 4px 6px rgba(0, 0, 0, 0.1) over 200ms ease-out.

**Form Elements**:
Design input fields with tertiary background, 1px border using border color, 6px border radius, and 12px vertical, 16px horizontal padding. Labels should be positioned above inputs with 8px bottom margin using form label specifications. Required fields should have a red asterisk.

**Button System**:
Primary buttons: primary accent background, white text, 6px border radius, button text specifications, and interaction animations above. Secondary buttons: transparent background, primary accent border and text, same padding and radius. Disabled buttons: slate-300 background, slate-500 text, no interactions.

**Data Tables**:
Create tables with white background and 1px borders using border color. Header cells should have secondary background with table header text specifications and 12px vertical, 16px horizontal padding. Data cells should use primary text with same padding. Alternate row backgrounds using tertiary background for improved scanning.

**Status Indicators**:
Design alert components with appropriate status backgrounds and borders, 8px border radius, 16px padding, and relevant text colors. Include icons aligned to the left with 8px right margin. Dismissible alerts should have a close button in the top-right corner.

### Accessibility Specifications

**Color Contrast Compliance**:
Ensure all text meets WCAG AA standards with minimum 4.5:1 contrast ratio for normal text and 3:1 for large text. Primary text on white background provides 16.7:1 ratio. Secondary text provides 7.2:1 ratio.

**Focus Management**:
Implement visible focus indicators for all interactive elements using primary accent color with 2px outline and 2px offset. Focus should move logically through the interface following tab order.

**Screen Reader Support**:
Include proper ARIA labels, roles, and states for all interactive components. Form fields should have associated labels and error messages. Status updates should be announced to screen readers.

**Keyboard Navigation**:
Ensure all functionality is accessible via keyboard with logical tab order. Dropdown menus should open with Enter or Space and navigate with arrow keys. Modal dialogs should trap focus and close with Escape.

### Responsive Behavior

**Breakpoint Strategy**:
- Mobile: 320px-767px (single column, stacked navigation, larger touch targets)
- Tablet: 768px-1199px (2-column layouts, condensed spacing)
- Desktop: 1200px+ (multi-column layouts, full feature set)

**Typography Scaling**:
Maintain heading hierarchy while reducing sizes by 15% on tablet and 25% on mobile. Increase body text line height to 1.65 on mobile for improved readability.

**Navigation Adaptation**:
On desktop, display full horizontal navigation. On tablet, maintain horizontal layout but reduce spacing. On mobile, implement a clean hamburger menu that reveals navigation items in a full-screen overlay with fade-in animation.

**Component Responsiveness**:
Cards should maintain 24px padding on desktop, reduce to 20px on tablet, and 16px on mobile. Tables should become horizontally scrollable on mobile with sticky first columns. Forms should stack vertically on mobile with full-width inputs.

**Touch Optimization**:
Increase minimum touch target size to 48px on mobile devices. Add 8px margin between interactive elements. Implement touch feedback with subtle opacity changes on tap. 