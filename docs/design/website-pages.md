# Kaevo AI — Website Pages UX Blueprint

> Page-by-page user experience and layout specifications for the public-facing Kaevo AI website.

---

## 1. Homepage (`/`)

### Purpose
To establish Kaevo AI as the premier AI automation agency, immediately demonstrate value (via the AI Receptionist), and drive visitors toward qualification or booking.

### Layout & Sections
1. **Global Navigation:** Logo left, Links (Services, Portfolio, About), CTA (Book Strategy Call) right. Sticky on scroll.
2. **Hero Section:**
   - **Headline:** Bold, value-driven (e.g., "Automate your business with intelligent AI employees.")
   - **Subheadline:** "Kaevo AI builds custom AI systems that save you 20+ hours a week."
   - **Primary CTA:** "Book a Strategy Call" (Primary Button)
   - **Visual:** Abstract 3D/glassmorphic representation of an AI brain or workflow.
3. **Social Proof (Trust Bar):** Logos of integrated technologies (OpenAI, Anthropic) or client logos.
4. **Services Overview (Cards):** 3-column grid highlighting core offerings (AI Chatbots, Automation Workflows, Outbound Engines).
5. **Interactive Demo (Value Prop):** A visual breakdown of "Before Kaevo" vs "After Kaevo" using a slider or simple animation.
6. **Testimonial / Case Study Highlight:** Quote from a successful client + metric (e.g., "Saved 40 hrs/week").
7. **Final CTA:** Full-width section pushing to the qualification flow or booking.
8. **Global Footer:** Links, legal, social icons, newsletter signup.

### UX Notes
- **AI Chat Widget:** Prominently bounces/activates 5 seconds after page load to greet the user (Agent A1).
- **Scroll Effects:** Use subtle fade-in-up animations as sections enter the viewport.

---

## 2. Services Page (`/services`)

### Purpose
To detail the specific AI solutions Kaevo provides, educate the user on the technology, and justify the investment.

### Layout & Sections
1. **Hero Header:** Simple title ("Our AI Solutions") and brief description.
2. **Service Deep Dives (Alternating Layout):**
   - **Block A:** Image/Graphic left, Text right (e.g., "AI Customer Support"). Include bulleted features and a "Learn More" tertiary button.
   - **Block B:** Text left, Image/Graphic right (e.g., "Outbound Sales Engine").
3. **Pricing / Tiers (Optional):** 3-column pricing cards if pricing is public. Highlight the "Recommended" tier.
4. **Technology Stack:** Grid of logos showing what powers the services (GPT-4, Claude, LangChain).
5. **Bottom CTA:** "Not sure what you need? Let our AI qualify you." (Triggers Agent A2).

### Components
- **Feature Checklists:** Use the Emerald Success icon for positive feature affirmations.
- **Service Cards:** Interactive cards that elevate on hover.

---

## 3. About Page (`/about`)

### Purpose
To build trust, tell the Kaevo AI story, and introduce the vision of evolving into a global AI SaaS.

### Layout & Sections
1. **Mission Hero:** Large, impactful statement of the Kaevo AI vision and mission.
2. **The Story:** 2-column text layout or text with a timeline graphic detailing the journey from Agency to SaaS.
3. **Core Values:** 3-column icon grid (e.g., "Frictionless", "Intelligent", "Scalable").
4. **The Team:** Profile cards for founders/key members (photo, name, role, brief bio).
5. **Careers / Join Us:** Banner for open positions.

### UX Notes
- Focus on typography and generous whitespace. The tone should feel highly professional and visionary.

---

## 4. Contact / Book Meeting (`/book`)

### Purpose
Frictionless scheduling of strategy calls for highly qualified leads.

### Layout & Sections
1. **Split Screen Layout (Desktop):**
   - **Left Column:** "Book your free strategy call." Value proposition bullet points (what to expect on the call), and a brief testimonial.
   - **Right Column:** Embedded Calendar widget (Cal.com / Calendly) in a clean, elevated card.
2. **Alternative Contact:** Below the calendar, links to "Email us directly" or "Support".

### UX Notes
- Ensure the embedded calendar styling matches the Kaevo dark theme.
- Hide standard website navigation (header/footer) to create a focused, distraction-free "funnel" page.

---

## 5. AI Chat / Qualification Standalone (`/chat` or `/qualify`)

### Purpose
A dedicated, full-screen conversational interface for users who click "Get a Quote" or "Find my Solution".

### Layout & Sections
1. **Full-Screen Chat Interface:** 
   - Left sidebar (optional): Step indicator (e.g., 1. Business Info, 2. Challenges, 3. Solution).
   - Main area: Chat feed similar to ChatGPT/Claude interfaces.
   - Bottom: Input field with suggestion chips.

### UX Notes
- Refer to [AI Chat Experience](ai-chat-experience.md) for detailed interaction mechanics.
- Auto-focus the input field on load.

---

## 6. 404 Error Page (`/404`)

### Purpose
To gracefully handle lost users and route them back to the primary funnel.

### Layout & Sections
1. **Central Graphic:** Playful/futuristic illustration (e.g., a disconnected robot or a glowing empty portal).
2. **Header:** "404 - Neural link severed." or "Page not found."
3. **Subtext:** "The page you're looking for doesn't exist in our current model."
4. **Action:** Primary button ("Return Home") and Secondary button ("Ask our AI Assistant").

### UX Notes
- Keep it lighthearted but on-brand.
- Ensure the AI chat widget is still accessible here so the user can just ask the bot where to go.
