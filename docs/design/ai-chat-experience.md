# Kaevo AI — AI Chat Experience

> Design guidelines for the conversational UI powered by Agent A1 (Receptionist) and Agent A2 (Qualification).

---

## 1. Chat Layout & Positioning

### Widget State (System 1 Receptionist)
- **Position:** Fixed, bottom-right corner.
- **Collapsed:** A floating action button (FAB) containing the Kaevo AI abstract avatar. Subtle glowing pulse animation to attract attention.
- **Expanded:** A rectangular panel (e.g., 380px wide, 600px high).
  - **Header:** Brand primary color background. Avatar, Name ("Kaevo"), Status ("Online"), and a close/minimize button.
  - **Body:** Scrollable message feed. Dark surface background (`color-bg-base`).
  - **Footer:** Input field, attachment icon (if applicable), and send button.

### Full-Screen State (Dedicated Qualification/Booking)
- **Position:** Centered column, max-width 768px, covering the full viewport height.
- **Header:** Clean logo centered at the top.
- **Focus:** No distractions, just the conversation flow.

---

## 2. Conversational UI Components

### Message Bubbles
- **AI Messages (Left):** 
  - Avatar displayed next to the bubble.
  - Background: `color-surface-2`.
  - Text: `color-text-primary`.
  - Corners: Rounded `radius-lg`, except the top-left corner (`radius-sm`).
- **User Messages (Right):**
  - No avatar needed.
  - Background: `color-brand-primary` (Electric Blue).
  - Text: `color-text-inverse` (White/Light).
  - Corners: Rounded `radius-lg`, except the top-right corner (`radius-sm`).

### Suggestion Chips
- **Purpose:** Provide frictionless 1-click answers to common questions (e.g., "Pricing", "Book a Call", "What is AI Automation?").
- **Style:** Pill-shaped (`radius-full`), ghost button style (border only).
- **Interaction:** Hover changes border to brand color. Clicking a chip automatically sends that text as a user message.
- **Placement:** Horizontally scrolling row right above the input field.

### AI Typing Indicator
- **Visual:** Three small dots pulsing sequentially.
- **Purpose:** Prevents user frustration by indicating the LLM is generating a response.
- **Duration:** Shown immediately after user sends a message, hidden the millisecond the AI stream begins.

---

## 3. Rich Interactive Cards

Instead of just plain text, the AI can render rich UI components inside the chat feed.

### Service Recommendation Card (Agent A2 output)
- Triggered when the AI recommends a specific service.
- **Layout:**
  - Icon/Graphic representing the service.
  - Title (e.g., "AI Voice Assistant").
  - 2-3 bullet points of value (e.g., "Answers calls 24/7", "Books directly to calendar").
  - Button: "Select this solution" or "Learn More".
- **Visual:** Embedded inside an AI message bubble, uses standard Card design tokens.

### Meeting Booking Interface
- Triggered when the user intends to book a strategy call.
- **Layout:** 
  - Instead of sending the user to a new page, a mini-calendar (Date picker + Time slot pills) renders directly *inside* the chat flow.
  - Once selected, the AI confirms: "Great, I've booked you for Tuesday at 2 PM. An invite is in your inbox."

---

## 4. Conversation Flow UX Principles

1. **Progressive Disclosure:** Don't ask 5 qualification questions in one massive text block. Ask one question at a time.
2. **Contextual Memory:** If the user provided their company name in a previous message, the AI should use it naturally (e.g., "How large is the team at Acme Corp?").
3. **Graceful Fallback:** If the user asks a complex question the AI cannot answer confidently, it should gracefully offer human handoff: *"That's a great question about custom ERP integration. I want to make sure you get the exact technical details. Shall I connect you with one of our lead architects?"*
4. **Tone:** Professional, helpful, slightly enthusiastic, and highly intelligent. (Never robotic).
