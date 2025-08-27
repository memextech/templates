# Y2K Retro Tech

### Overview
A nostalgic yet modern design system inspired by early 2000s digital aesthetics, featuring bold gradients, metallic textures, and futuristic elements. Perfect for creative agencies, tech startups, and consumer-facing products that want to stand out with memorable, personality-driven design.

### Typography Specifications

**Primary Font Pairing**: Use Orbitron for headings (futuristic feel) and Inter for body text (modern readability).

**Heading Typography**:
- H1: Orbitron Bold (700 weight) at 52px (3.25rem) with 0.02em letter spacing and text-transform: uppercase
- H2: Orbitron SemiBold (600 weight) at 38px (2.375rem) with 0.015em letter spacing
- H3: Orbitron Medium (500 weight) at 28px (1.75rem) with 0.01em letter spacing
- H4: Orbitron Medium (500 weight) at 22px (1.375rem) with 0.005em letter spacing

**Body Typography**:
- Primary body text: Inter Regular (400 weight) at 16px (1rem) with 1.6 line height
- Secondary body text: Inter Regular (400 weight) at 14px (0.875rem) with 1.55 line height
- Caption text: Inter Medium (500 weight) at 12px (0.75rem) with 1.5 line height and text-transform: uppercase

**Special Typography**:
- Button text: Orbitron SemiBold (600 weight) at 14px with 0.05em letter spacing and text-transform: uppercase
- Navigation text: Orbitron Medium (500 weight) at 15px with 0.02em letter spacing
- Code/tech text: JetBrains Mono Regular (400 weight) at 14px with neon green color accent

### Color Palette Specifications

**Background System**:
- Primary background: Radial gradient from #0a0a0a (center) to #1a0033 (edges) for deep space effect
- Secondary background: Linear gradient from #1a0033 to #000814 at 45-degree angle
- Card backgrounds: Linear gradient from rgba(255, 255, 255, 0.05) to rgba(255, 255, 255, 0.02)
- Overlay backgrounds: rgba(0, 8, 20, 0.9) with subtle noise texture

**Neon Accent System**:
- Primary neon: #00ffff (cyan) with glow effect: 0 0 20px #00ffff, 0 0 40px #00ffff
- Secondary neon: #ff00ff (magenta) with glow effect: 0 0 20px #ff00ff, 0 0 40px #ff00ff
- Tertiary neon: #00ff00 (lime green) with glow effect: 0 0 20px #00ff00, 0 0 40px #00ff00
- Warning neon: #ffff00 (yellow) with glow effect: 0 0 20px #ffff00, 0 0 40px #ffff00
- Error neon: #ff0080 (hot pink) with glow effect: 0 0 20px #ff0080, 0 0 40px #ff0080

**Metallic Gradients**:
- Chrome gradient: linear-gradient(135deg, #c0c0c0, #f0f0f0, #c0c0c0, #a0a0a0)
- Gold gradient: linear-gradient(135deg, #ffd700, #ffed4e, #ffd700, #b8860b)
- Holographic gradient: linear-gradient(45deg, #ff00ff, #00ffff, #ff00ff, #00ffff)

**Text Colors**:
- Primary text: #ffffff with subtle text-shadow: 0 0 10px rgba(255, 255, 255, 0.5)
- Secondary text: #b0b0b0 with text-shadow: 0 0 5px rgba(176, 176, 176, 0.3)
- Accent text: Use neon colors with appropriate glow effects
- Disabled text: #404040 with no glow effects

### Visual Effects Specifications

**Glow Effects**:
Apply neon glow effects to interactive elements using box-shadow with multiple layers:
- Inner glow: inset 0 0 10px rgba(neon-color, 0.3)
- Outer glow: 0 0 20px rgba(neon-color, 0.6), 0 0 40px rgba(neon-color, 0.4)
- Distant glow: 0 0 80px rgba(neon-color, 0.2)

**Holographic Effects**:
Create holographic surfaces using background: linear-gradient(45deg, transparent 30%, rgba(255, 255, 255, 0.1) 50%, transparent 70%), linear-gradient(135deg, #ff00ff, #00ffff, #ff00ff) with background-size: 200% 200% and animated background-position.

**Scan Line Animation**:
Implement retro scan line effects using pseudo-elements with background: repeating-linear-gradient(0deg, transparent, transparent 2px, rgba(0, 255, 255, 0.1) 2px, rgba(0, 255, 255, 0.1) 4px) and animated transform: translateY() over 2 seconds.

**Pixelation Effects**:
For retro elements, apply image-rendering: pixelated and use 8-bit style borders with box-shadow: 2px 2px 0 #00ffff, 4px 4px 0 #ff00ff for layered pixel effect.

### Layout and Spacing

**Grid System with Perspective**:
Use a 12-column grid with perspective transforms for depth. Apply transform: perspective(1000px) rotateX(5deg) to container elements for subtle 3D effect.

**Spacing with Neon Emphasis**:
- Component spacing: 32px between major sections with neon divider lines
- Card spacing: 24px with glowing borders
- Element padding: 20px with inner glow effects
- Button spacing: 16px with outer glow margins

**Retro Container Styles**:
Create containers with background: linear-gradient(135deg, rgba(255, 255, 255, 0.1), rgba(255, 255, 255, 0.05)), border: 2px solid #00ffff, border-radius: 0 (sharp corners for retro feel), and box-shadow glow effects.

### Animation and Interaction

**Neon Animation Timing**:
- Glow pulse: 2 seconds ease-in-out infinite alternate
- Color cycling: 4 seconds linear infinite
- Hover transitions: 300ms ease-out
- Loading animations: 1.5 seconds ease-in-out infinite

**Interactive Glow Effects**:
On hover, intensify glow effects by increasing box-shadow blur radius by 50% and opacity by 30%. Add a subtle scale transform: scale(1.05) over 300ms ease-out.

**Retro Loading Animations**:
Create loading states with animated dots using neon colors, pulsing at 0.5-second intervals with increasing glow intensity. Include retro progress bars with animated gradient fills.

**Text Glitch Effects**:
For special headings, implement glitch animations using text-shadow with rapid color changes between cyan and magenta over 100ms intervals, triggered on hover or focus.

**Holographic Rotation**:
Apply continuous rotation animations to holographic elements using transform: rotate() over 10 seconds linear infinite, creating mesmerizing visual effects.

### Component Specifications

**Neon Navigation**:
Design navigation with background: rgba(0, 0, 0, 0.8), border-bottom: 2px solid #00ffff with glow effect, height: 80px. Navigation items should have neon text with glow effects and animated underlines that draw in from left to right on hover.

**Retro Cards**:
Create cards with sharp corners (border-radius: 0), neon borders cycling through color spectrum, and background gradients with scan line overlays. Include retro-style headers with uppercase Orbitron text and neon accents.

**Futuristic Buttons**:
Design buttons with background: linear-gradient(135deg, #00ffff, #0080ff), text-transform: uppercase, border: none, border-radius: 0, and multi-layer glow effects. Hover state should intensify glow and add subtle transform: skew(-5deg).

**Holographic Forms**:
Create form elements with transparent backgrounds, neon borders, and holographic placeholder text. Focus states should activate intense glow effects and animated border colors cycling through the spectrum.

**Retro Data Displays**:
Design data tables and charts with green monospace text on dark backgrounds, reminiscent of old computer terminals. Include animated cursor effects and typing animations for data reveals.

**Neon Status Indicators**:
Create status badges with appropriate neon colors, pulsing glow animations, and uppercase text. Success states use lime green, warnings use yellow, errors use hot pink, all with matching glow effects.

### Responsive Behavior

**Mobile Neon Optimization**:
Reduce glow effects intensity by 40% on mobile devices for performance. Simplify animations and use CSS transforms instead of complex box-shadows where possible.

**Touch-Friendly Neon Elements**:
Increase touch targets to 56px minimum with 12px spacing between interactive elements. Maintain neon aesthetic while ensuring usability on touch devices.

**Adaptive Glow Effects**:
On lower-powered devices, provide fallback styles with solid neon colors instead of glow effects. Use media queries to detect device capabilities and adjust accordingly.

**Retro Responsive Patterns**:
Maintain the Y2K aesthetic across all screen sizes by scaling effects proportionally. On mobile, stack elements vertically while preserving neon borders and glow effects at reduced intensity.

### Performance Considerations

**Optimized Glow Rendering**:
Use will-change: transform, box-shadow on animated elements to optimize rendering. Limit simultaneous glow animations to prevent performance issues.

**Efficient Color Cycling**:
Implement color cycling using CSS custom properties and single keyframe animations rather than multiple simultaneous animations.

**Reduced Motion Support**:
Provide prefers-reduced-motion alternatives that maintain the Y2K aesthetic without intensive animations, using static neon colors and simplified effects. 