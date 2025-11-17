# J&M Design Guidelines

## Design Approach
**Reference-Based + Custom Romantic Aesthetic**: Drawing inspiration from Notion's clean editor patterns and Spotify's media player UI, combined with a unique romantic, playful aesthetic featuring soft animations and intimate interactions.

## Core Design Principles
1. **Emotional Connection**: Every interaction should feel personal and meaningful
2. **Playful Elegance**: Dark themes with vibrant accents, soft particles, gentle animations
3. **Seamless Real-time**: Live updates feel natural, not jarring
4. **Intimate Scale**: Design for two users, not thousands

---

## Theme System

### Theme J (Jéssica)
- **Base**: Pure black (#000000) backgrounds
- **Primary**: Soft neon green (#00FF88, #10B981 variants)
- **Secondary**: Pastel pink (#FFB6C1, #FF69B4 variants)
- **Accents**: Gentle gradient overlays, green-to-pink transitions
- **Particles**: Pink heart-shaped particles, green sparkles

### Theme M (Miguel)
- **Base**: Pure black (#000000) backgrounds
- **Primary**: Deep red (#DC2626, #B91C1C variants)
- **Secondary**: Darker crimson (#7F1D1D)
- **Accents**: Red glows, subtle fire-like gradients
- **Particles**: Red hearts, ember-like floating elements

### Shared Elements
- Both themes use dark mode exclusively
- White text (#FFFFFF) for primary content, gray (#9CA3AF) for secondary
- Smooth theme transitions (300ms ease)
- Theme toggle in header with animated icon swap

---

## Typography

**Font Families**:
- Primary: "Inter" (clean, modern) via Google Fonts
- Cute/Playful: "Quicksand" for headers and special elements
- Monospace: "JetBrains Mono" for code blocks in Notes

**Hierarchy**:
- Hero/Page Titles: text-5xl md:text-6xl, font-bold, Quicksand
- Section Headers: text-3xl md:text-4xl, font-semibold
- Card Titles: text-xl font-medium
- Body: text-base leading-relaxed
- Captions: text-sm text-gray-400
- Buttons: text-sm font-medium uppercase tracking-wide

---

## Layout & Spacing

**Spacing System**: Use Tailwind units of **2, 4, 8, 12, 16, 20** (e.g., p-4, m-8, gap-12)

**Container Strategy**:
- Page wrapper: max-w-7xl mx-auto px-4 sm:px-6 lg:px-8
- Content sections: py-12 md:py-20
- Cards/Components: p-6 md:p-8
- Tight spacing for post-its/drawing tools: gap-2, p-2

**Grid Patterns**:
- Photo/Cartinhas galleries: grid-cols-2 md:grid-cols-3 lg:grid-cols-4 gap-4
- Feature cards: grid-cols-1 md:grid-cols-2 gap-8
- Post-it mural: Free-form absolute positioning
- Drawing toolbar: Horizontal flex with gap-2

---

## Component Library

### Navigation
- Fixed header with blur backdrop (backdrop-blur-lg bg-black/80)
- Logo/site name left, navigation center, user greeting + theme toggle right
- Mobile: Hamburger menu with slide-in drawer
- Active page indicator with theme-colored underline

### Cards
- Rounded corners: rounded-xl
- Subtle border: border border-gray-800
- Hover lift: hover:scale-105 transition-transform
- Shadow on hover: hover:shadow-2xl hover:shadow-[theme-color]/20

### Modals/Overlays
- Full-screen overlay: bg-black/90 backdrop-blur-sm
- Modal container: max-w-4xl bg-gray-900 rounded-2xl p-8
- Close button: Absolute top-right, X icon with hover:rotate-90 transition

### Forms & Inputs
- Input fields: bg-gray-900 border border-gray-700 rounded-lg px-4 py-3
- Focus state: focus:ring-2 focus:ring-[theme-color] focus:border-transparent
- Buttons: rounded-full px-6 py-3 with theme colors, hover:brightness-110
- File upload: Drag-drop zone with dashed border, hover state

### Post-its
- Size: w-48 h-48 (fixed for consistency)
- Shadow: shadow-lg with rotation transform
- Draggable cursor indicator
- 8 color options: yellow, pink, blue, green, purple, orange, red, white
- 3 font options: Inter, Quicksand, Indie Flower

### Drawing Canvas
- Full-width canvas with aspect ratio constraint
- Toolbar: Fixed bottom with tools in horizontal row
- Color palette: Circle swatches, active state with ring
- Brush size: Range slider with visual preview

### Music Player
- Floating bottom-right: fixed bottom-4 right-4
- Compact card: w-80 bg-gray-900/95 backdrop-blur-lg rounded-2xl
- Album art placeholder, controls row, progress bar
- Minimize/expand toggle

---

## Animations & Interactions

**Micro-interactions** (Framer Motion):
- Card hover: scale(1.05) + shadow
- Button press: scale(0.98)
- Page transitions: Fade + slide up (y: 20 to 0)
- Modal open: Scale from 0.9 to 1 + fade in

**Background Particles**:
- Canvas-based, 20-30 particles max
- Slow float animation, subtle scale pulse
- Theme-colored with 0.3-0.6 opacity
- Performance: requestAnimationFrame, 60fps cap

**Sound Effects** (subtle):
- Button click: Soft pop (50ms)
- Message send: Gentle whoosh
- Photo upload: Camera shutter
- Counter hover: Tiny bell chime
- All sounds <100ms duration, volume 0.3

**Hover States**:
- Counter date: Glow effect + particle burst + increment animation
- Photo thumbnails: Zoom + brightness increase
- Post-its: Subtle lift + rotate(2deg)

---

## Page-Specific Layouts

### Contador (Counter)
- Centered hero layout
- Large animated numbers: text-6xl md:text-8xl font-bold
- Hover interaction zone around date
- Touch counter badge: Floating top-right corner
- Particle burst on hover interaction

### Cartinhas & Nossas Fotos
- Grid gallery with masonry option for photos
- Upload button: Fixed bottom-right FAB (Floating Action Button)
- Thumbnail size: 200x200px with object-cover
- Modal: Center-screen with PDF viewer embed or full-size image

### Mensagens (Post-its)
- Full-screen mural: min-h-screen bg with particle layer
- Create button: Fixed top-right
- Post-its: Draggable with rotation variance (-3deg to 3deg)
- Color picker modal: Grid of color swatches

### Desenho Online
- Canvas: Full width with max-height, aspect-ratio: 16/9
- Toolbar: Horizontal bottom bar with tool icons
- Undo/redo: Top-right corner buttons
- Save button: Prominent top-right, theme-colored

### Notas (Notes)
- Sidebar: Left 240px with note list (collapsible on mobile)
- Editor: Rich content area with Tiptap toolbar
- Notion-style: Slash commands, block formatting
- Title: Large editable text-4xl at top

---

## Images & Media

**Hero Sections**: No traditional hero images; use animated backgrounds with particles and typography

**Photo Galleries**:
- Thumbnails: 200x200px, rounded-lg, object-cover
- Hover preview: Scale(1.1) with shadow
- Full-size modal: Contain with max-w-screen-xl

**PDF Thumbnails**:
- Generate 200x200px preview of first page
- Fallback: PDF icon with filename overlay

**Music Player**:
- Album art: 80x80px rounded square
- Waveform visualization: Optional canvas animation

---

## Responsive Breakpoints

- Mobile: < 640px (single column, stacked navigation)
- Tablet: 640px - 1024px (2-column grids, side drawer)
- Desktop: > 1024px (multi-column, full features)

**Mobile Adaptations**:
- Post-its: Smaller size (w-32 h-32)
- Drawing canvas: Touch-optimized, larger brush controls
- Music player: Bottom sheet instead of floating card

---

## Accessibility

- All interactive elements: min 44px touch target
- Focus indicators: Visible ring in theme color
- Alt text for all uploaded images
- ARIA labels for icon-only buttons
- Keyboard navigation for drawing tools and post-its
- Screen reader announcements for real-time updates