# Kaevo AI — Design System

> Core UI components and visual guidelines for the Kaevo AI platform.

---

## 1. Vision & Aesthetic

The Kaevo AI design language is built on three core pillars:
1. **Intelligent & Futuristic:** Deep contrasts, subtle glows, and micro-animations that feel "alive."
2. **Enterprise Trust:** Clean lines, highly legible typography, and structured layouts that convey reliability.
3. **Frictionless UX:** Generous spacing, clear affordances, and zero cognitive overload.

**Theme:** A default "Dark Mode First" aesthetic, featuring a deep obsidian background with vibrant electric blue and violet accents to symbolize AI processing and intelligence.

---

## 2. Color Palette

### Primary (Brand Identity)
- **Primary Brand (Electric Blue):** For primary actions, AI active states, and brand highlights.
- **Secondary Brand (Deep Violet):** For gradients, subtle accents, and secondary AI indicators.

### Surface & Background (Dark Theme)
- **Background Base:** Very dark gray/blue (almost black) — provides depth without harsh contrast.
- **Surface Level 1:** Slightly lighter gray — for cards and main content areas.
- **Surface Level 2:** Lighter still — for elevated elements like dropdowns and modals.

### Status & Semantic
- **Success (Emerald):** Positive actions, completed tasks, high scores.
- **Warning (Amber):** Pending items, caution states.
- **Danger (Crimson):** Destructive actions, errors, low scores.
- **Info (Sky Blue):** Informational banners, helpful tips.

> *For specific hex values and CSS variables, refer to [Design Tokens](design-tokens.md).*

---

## 3. Typography

Kaevo AI uses a two-tier typography system for a modern SaaS look.

### Primary Typeface: Inter (or Plus Jakarta Sans)
- **Usage:** Headings, UI text, navigation, and data tables.
- **Characteristics:** Highly legible geometric sans-serif, excellent at small sizes for dashboards.

### Secondary Typeface: JetBrains Mono (or Fira Code)
- **Usage:** Code snippets, IDs, API keys, and data visualization labels.
- **Characteristics:** Monospaced, tech-forward, reinforces the engineering quality of the platform.

### Typographic Scale
- Clear hierarchy from `H1` (page titles) down to `Caption` (metadata).
- High contrast in font weights (e.g., Semibold for headers, Regular for body text).

---

## 4. Buttons

Buttons follow a strict hierarchy and distinct visual styles.

### Styles
- **Primary:** Solid background (Electric Blue), no border, glowing shadow on hover. Used for the main action on a page.
- **Secondary:** Transparent background, solid border (Subtle Gray), text matches border color (changes to brand color on hover). Used for alternative actions.
- **Tertiary / Ghost:** No background, no border, text color changes on hover. Used for low-priority actions (e.g., "Cancel").
- **Destructive:** Solid background (Crimson) or Ghost with Crimson text. For deletions and irreversible actions.

### States
- **Default:** Standard appearance.
- **Hover:** Slight scale up (1.02x), increased brightness or shadow.
- **Active / Pressed:** Scale down (0.98x), reduced brightness.
- **Disabled:** 50% opacity, cursor-not-allowed, no hover effects.
- **Loading:** Text replaced or accompanied by a spinning loader icon.

---

## 5. Forms & Inputs

Inputs are designed for high legibility and clear interaction states.

### Text Inputs & Textareas
- **Style:** Subtle dark background, minimal border.
- **Focus State:** Border changes to Primary Brand color, subtle outer glow.
- **Error State:** Border changes to Danger color, error message appears below.
- **Labels:** Positioned above the input, small and muted color.
- **Placeholders:** Muted text indicating expected format.

### Selections (Checkboxes, Radios, Toggles)
- **Toggles:** Preferred over checkboxes for immediate system state changes (e.g., enabling an AI Agent).
- **Checkboxes:** Used for multi-select in lists or forms.
- **Radios:** Used for mutually exclusive options (e.g., selecting a pricing tier).

---

## 6. Cards & Containers

Cards are the primary building blocks for dashboards and content grouping.

### Visual Style
- **Background:** Surface Level 1.
- **Border:** Very subtle, 1px semi-transparent border (glassmorphism effect).
- **Border Radius:** Medium rounded corners (e.g., 12px) for a soft, modern feel.
- **Padding:** Generous internal padding (e.g., 24px) to let content breathe.

### Interactive Cards
- Used for selectable services, agent profiles, or dashboard widgets.
- **Hover State:** Slight upward translation (-2px), increased shadow (elevation), border brightens.

---

## 7. Icons & Imagery

### Iconography
- **Library:** Phosphor Icons or Heroicons (Outline style preferred).
- **Stroke Weight:** Consistent 1.5px or 2px stroke.
- **Usage:** Accompanying text labels in navigation, buttons, and empty states. Never use icons alone without a tooltip.

### Imagery & Avatars
- **Agent Avatars:** Abstract, geometric, or glowing representations of the AI agents.
- **User Avatars:** Standard circular profile pictures.
- **Illustrations:** Minimalist, wireframe, or isometric tech illustrations for empty states and landing pages.

---

## 8. Shadows & Elevation

Shadows are used sparingly in the dark theme to create depth (elevation).

- **Level 1 (Cards, Inputs):** Very subtle, tight shadow.
- **Level 2 (Dropdowns, Tooltips):** Softer, more diffuse shadow.
- **Level 3 (Modals, Popovers):** Large, soft shadow with a slight color tint (brand color) to create a glowing effect.

---

## 9. Grid System & Spacing

### Grid
- **Desktop:** 12-column grid, 24px gutters, max-width 1440px.
- **Tablet:** 8-column grid, 16px gutters.
- **Mobile:** 4-column grid, 16px gutters.

### Spacing (The 8pt System)
- All margins and padding are multiples of 8 (8, 16, 24, 32, 48, 64, etc.).
- 4px is used for micro-spacing (e.g., between an icon and text).

---

## 10. Micro-Animations

Animations should be purposeful, providing feedback and guiding the user's attention.

- **Transitions:** Fast and smooth (150ms - 200ms) with ease-out timing functions for UI state changes (hover, focus).
- **Page Loads:** Staggered fade-ins (slide up + opacity) for lists and dashboard widgets.
- **AI Processing:** "Skeleton" loaders or pulsating glowing borders to indicate the AI is "thinking" or processing data.
- **Success States:** Brief, satisfying animations (e.g., a checkmark drawing itself).
