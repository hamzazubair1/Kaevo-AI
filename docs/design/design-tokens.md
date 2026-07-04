# Kaevo AI — Design Tokens

> Reusable design variables mapping core values for colors, typography, spacing, radius, elevation, and motion. 
> These tokens are intended to be translated directly into CSS Variables (or Tailwind config).

---

## 1. Color Tokens (Dark Theme Default)

### Core Brand
| Token Name | Hex Value | Usage |
| :--- | :--- | :--- |
| `color-brand-primary` | `#3B82F6` (Blue 500) | Primary buttons, active states, highlights |
| `color-brand-primary-hover` | `#2563EB` (Blue 600) | Primary button hover |
| `color-brand-secondary` | `#8B5CF6` (Violet 500) | Gradients, secondary AI indicators |
| `color-brand-secondary-hover`| `#7C3AED` (Violet 600) | Secondary action hover |

### Backgrounds & Surfaces
| Token Name | Hex Value | Usage |
| :--- | :--- | :--- |
| `color-bg-base` | `#0B0F19` (Very Dark Navy) | Main app background (body) |
| `color-surface-1` | `#111827` (Gray 900) | Cards, panels, sidebars |
| `color-surface-2` | `#1F2937` (Gray 800) | Modals, dropdowns, hovered cards |
| `color-surface-3` | `#374151` (Gray 700) | Borders, dividers, disabled states |

### Text & Typography
| Token Name | Hex Value | Usage |
| :--- | :--- | :--- |
| `color-text-primary` | `#F9FAFB` (Gray 50) | Headings, primary body text |
| `color-text-secondary` | `#9CA3AF` (Gray 400) | Subtitles, labels, secondary info |
| `color-text-muted` | `#6B7280` (Gray 500) | Placeholders, disabled text, meta |
| `color-text-inverse` | `#111827` (Gray 900) | Text on primary brand background |

### Semantic / Status
| Token Name | Hex Value | Usage |
| :--- | :--- | :--- |
| `color-status-success` | `#10B981` (Emerald 500) | Success messages, high scores |
| `color-status-warning` | `#F59E0B` (Amber 500) | Warnings, pending states |
| `color-status-danger` | `#EF4444` (Red 500) | Errors, destructive actions |
| `color-status-info` | `#0EA5E9` (Sky 500) | Informational badges |

---

## 2. Typography Tokens

### Font Families
| Token Name | Value | Description |
| :--- | :--- | :--- |
| `font-family-sans` | `'Inter', system-ui, sans-serif` | Primary typeface for all UI |
| `font-family-mono` | `'JetBrains Mono', monospace` | Code, metrics, API keys |

### Font Sizes (Base 16px)
| Token Name | Rem Value | Pixel Equiv | Usage |
| :--- | :--- | :--- | :--- |
| `font-size-xs` | `0.75rem` | 12px | Captions, small labels |
| `font-size-sm` | `0.875rem`| 14px | Secondary text, table data |
| `font-size-base` | `1rem` | 16px | Standard body text |
| `font-size-lg` | `1.125rem`| 18px | Large body, small headers |
| `font-size-xl` | `1.25rem` | 20px | H4, Card titles |
| `font-size-2xl` | `1.5rem` | 24px | H3, Section headers |
| `font-size-3xl` | `1.875rem`| 30px | H2, Page headers |
| `font-size-4xl` | `2.25rem` | 36px | H1, Hero titles |
| `font-size-5xl` | `3rem` | 48px | Display/Hero prominent |

### Font Weights
| Token Name | Value | Usage |
| :--- | :--- | :--- |
| `font-weight-regular` | `400` | Body text |
| `font-weight-medium` | `500` | Buttons, important data, subtitles |
| `font-weight-semibold`| `600` | Section headers, card titles |
| `font-weight-bold` | `700` | Page headers, Hero titles |

---

## 3. Spacing Tokens (8pt Grid System)

| Token Name | Rem Value | Pixel Equiv | Usage Example |
| :--- | :--- | :--- | :--- |
| `space-1` | `0.25rem` | 4px | Icon to text spacing |
| `space-2` | `0.5rem` | 8px | Tight list items |
| `space-3` | `0.75rem` | 12px | Small input padding |
| `space-4` | `1rem` | 16px | Standard padding, mobile margins |
| `space-6` | `1.5rem` | 24px | Card padding, desktop gutters |
| `space-8` | `2rem` | 32px | Section spacing (small) |
| `space-12` | `3rem` | 48px | Section spacing (medium) |
| `space-16` | `4rem` | 64px | Section spacing (large) |
| `space-24` | `6rem` | 96px | Hero section padding |

---

## 4. Border & Radius Tokens

### Border Radius
| Token Name | Value | Usage |
| :--- | :--- | :--- |
| `radius-sm` | `4px` | Checkboxes, small tags |
| `radius-md` | `8px` | Buttons, inputs, small cards |
| `radius-lg` | `12px`| Standard cards, modals |
| `radius-xl` | `16px`| Large containers, hero images |
| `radius-full`| `9999px`| Avatars, pill badges |

### Border Widths
| Token Name | Value | Usage |
| :--- | :--- | :--- |
| `border-width-1` | `1px` | Standard borders (cards, dividers) |
| `border-width-2` | `2px` | Focus rings, active states |

---

## 5. Elevation (Shadow) Tokens

*Note: In dark mode, shadows are less visible, so we rely on borders and subtle background color changes (Surface 1 -> 2) alongside glow effects.*

| Token Name | Value | Usage |
| :--- | :--- | :--- |
| `shadow-sm` | `0 1px 2px rgba(0,0,0,0.5)` | Buttons, small elevated items |
| `shadow-md` | `0 4px 6px -1px rgba(0,0,0,0.5)` | Cards, interactive elements |
| `shadow-lg` | `0 10px 15px -3px rgba(0,0,0,0.5)`| Dropdowns, popovers |
| `shadow-xl` | `0 20px 25px -5px rgba(0,0,0,0.6)`| Modals, dialogs |
| `shadow-glow` | `0 0 15px rgba(59,130,246,0.3)` | AI active state, focus rings |

---

## 6. Motion & Animation Tokens

### Durations
| Token Name | Value | Usage |
| :--- | :--- | :--- |
| `motion-duration-fast` | `150ms` | Hover states, color changes, toggles |
| `motion-duration-base` | `200ms` | Dropdown open/close, simple transitions |
| `motion-duration-slow` | `300ms` | Modal fade-in, page transitions |

### Easing (Timing Functions)
| Token Name | Value | Description |
| :--- | :--- | :--- |
| `motion-ease-in-out` | `cubic-bezier(0.4, 0, 0.2, 1)` | Default smooth transition |
| `motion-ease-out` | `cubic-bezier(0, 0, 0.2, 1)` | Entering the screen (snappy) |
| `motion-ease-in` | `cubic-bezier(0.4, 0, 1, 1)` | Exiting the screen |
| `motion-ease-bounce` | `cubic-bezier(0.175, 0.885, 0.32, 1.275)` | Playful/AI notification pop |
