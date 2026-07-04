# Kaevo AI — Accessibility (a11y) Guidelines

> Standards to ensure the Kaevo AI platform is usable by everyone, adhering to WCAG 2.1 AA standards.

---

## 1. Color & Contrast

Because Kaevo AI utilizes a Dark Theme as its primary aesthetic, contrast management is critical to ensure text doesn't become illegible against dark backgrounds.

- **Text Contrast Requirement:** 
  - Normal text must have a minimum contrast ratio of **4.5:1** against its background.
  - Large text (18pt and larger, or 14pt bold) must have a minimum contrast ratio of **3.0:1**.
- **UI Elements:** Meaningful graphical elements (like the boundary of an input field or a chart line) must have a contrast ratio of **3.0:1** against adjacent colors.
- **Color Independence:** Never use color alone to convey meaning. 
  - *Example:* An error state on a form input shouldn't just turn the border red (Danger color); it must also display a text error message or a warning icon.

---

## 2. Keyboard Navigation

The entire web application and dashboards must be fully operable via a keyboard without requiring a mouse.

- **Focus Indicators:** 
  - Native browser focus outlines should be customized to match the brand (e.g., a `2px` solid Electric Blue ring with an offset), but they must **never** be removed (`outline: none` without a custom fallback is strictly forbidden).
- **Tab Order:** 
  - Ensure logical DOM structure so the `Tab` key moves focus sequentially left-to-right, top-to-bottom.
- **Skip Links:** 
  - Implement a visually hidden "Skip to main content" link at the very top of the DOM that becomes visible when it receives keyboard focus, allowing users to bypass global navigation.
- **Interactive Elements:** 
  - Ensure custom components (like customized dropdowns or toggles) handle `Enter` and `Space` keypresses correctly, mimicking native `<button>` and `<input>` behaviors.

---

## 3. Screen Reader Support & ARIA

- **Semantic HTML:** Use proper native HTML5 elements (`<header>`, `<nav>`, `<main>`, `<article>`, `<button>`, `<a>`) rather than `<div>` with click handlers. Native elements provide built-in accessibility.
- **ARIA Labels:** 
  - Use `aria-label` or `aria-labelledby` for icon-only buttons (e.g., the hamburger menu or the close button on the chat widget).
- **Live Regions (Crucial for AI Chat):**
  - When the AI agent generates a new message in the chat interface, screen readers must be notified. 
  - Wrap the chat message container in an `aria-live="polite"` region so the screen reader announces new messages automatically without interrupting the user mid-sentence.
- **Loading States:**
  - When a dashboard or agent is processing, use `aria-busy="true"` on the relevant container to inform assistive technologies.

---

## 4. Forms and Error Handling

- **Explicit Labels:** Every input must have a visible `<label>` bound via the `for` attribute to the input's `id`. Placeholders are not substitutes for labels.
- **Error Identification:** If a form submission fails (e.g., in the qualification flow), focus should be programmatically moved to the first input with an error, and the error message must be read by the screen reader.

---

## 5. Reduced Motion

Micro-animations (defined in the Design System) enhance the experience, but they can trigger vestibular disorders for some users.

- **`prefers-reduced-motion`:** Implement CSS media queries to detect if the user has requested reduced motion at the OS level.
  - If true: Disable scaling, bouncing, and sliding animations. Replace them with instantaneous changes or very fast cross-fades.
  ```css
  @media (prefers-reduced-motion: reduce) {
    * {
      animation-duration: 0.01ms !important;
      animation-iteration-count: 1 !important;
      transition-duration: 0.01ms !important;
      scroll-behavior: auto !important;
    }
  }
  ```
