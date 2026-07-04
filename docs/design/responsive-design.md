# Kaevo AI — Responsive Design Guidelines

> Breakpoints and layout shift guidelines to ensure a seamless cross-device experience for the Kaevo AI platform.

---

## 1. Breakpoints

Kaevo AI uses a mobile-first approach, scaling up gracefully to large desktop displays.

| Device | Breakpoint (min-width) | Grid Columns | Gutters | Margins |
| :--- | :--- | :--- | :--- | :--- |
| Mobile (sm) | `320px` / `0px` | 4 | 16px | 16px |
| Tablet (md) | `768px` | 8 | 16px | 32px |
| Desktop (lg) | `1024px` | 12 | 24px | Auto (Center) |
| Ultrawide (xl)| `1440px` | 12 | 32px | Auto (Center) |

---

## 2. Navigation Layout Shifts

### Global Website Navigation
- **Desktop (`>= 1024px`):** Horizontal top bar. Logo left, inline text links center, primary CTA button right.
- **Tablet/Mobile (`< 1024px`):** Logo left, Hamburger menu icon right. Tapping the hamburger triggers a full-screen or side-drawer overlay with vertical links.

### Dashboard Navigation (Admin & Client)
- **Desktop (`>= 1024px`):** Persistent left-side vertical sidebar (250px width). Main content area takes up the remaining `calc(100vw - 250px)`.
- **Tablet (`768px - 1023px`):** Sidebar collapses to an icon-only vertical rail (e.g., 64px width) to save horizontal space. Tooltips reveal labels on hover.
- **Mobile (`< 768px`):** Sidebar disappears. Bottom navigation bar (similar to mobile apps) for top 4 core views, with a "More" menu for the rest.

---

## 3. Component Adaptations

### Grids and Cards
- **Services/Portfolio Grids:**
  - Desktop: 3 columns (`grid-cols-3`).
  - Tablet: 2 columns (`grid-cols-2`).
  - Mobile: 1 column (`grid-cols-1`, stack vertically).

### Data Tables (Dashboards)
- **Desktop:** Full traditional table layout.
- **Mobile:** Tables transform into a stacked card layout. Instead of columns, each row becomes a separate card containing key-value pairs (e.g., `Status: Qualified`). Horizontal scrolling tables should be avoided on mobile if possible.

### Forms & Inputs
- **Desktop:** Side-by-side inputs (e.g., First Name, Last Name on one row).
- **Mobile:** All inputs stack vertically to ensure maximum touch target width and readability.

---

## 4. AI Chat Widget Adaptation

- **Desktop:** Floating Action Button (FAB) expands into a floating panel (380px wide) in the bottom right corner, not obscuring main page content.
- **Mobile:** 
  - The FAB remains in the bottom right corner (ensure it sits above mobile browser safe areas).
  - When tapped, the chat interface expands to **full-screen**, behaving like a native messaging app (header at top, input fixed at bottom). 
  - This prevents awkward scrolling and keyboard overlap issues common in mobile web design.

---

## 5. Typography Scaling

To maintain legibility without overwhelming small screens, fluid typography or step-based scaling is applied:
- `H1` (Hero titles) scale down significantly on mobile (e.g., from 48px to 32px).
- Body text remains largely the same (16px base) to ensure readability across all devices. Avoid dipping below 14px for any critical text.
