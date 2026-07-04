# Kaevo AI — AI Interaction Layer

> Specialized frontend patterns for communicating with and rendering AI agent responses.

---

## 1. Streaming UI Logic

Unlike traditional REST APIs that wait for a complete response, LLMs generate text token by token. The frontend must handle this stream to provide a responsive, ChatGPT-like experience.

### Implementation Concept
1. The frontend uses the standard `fetch` API rather than Axios for streaming endpoints, as it provides native support for `ReadableStream`.
2. A custom hook (`useChatStream`) initiates the POST request.
3. It attaches a reader to the `response.body`.
4. As the stream yields chunks (Uint8Arrays), they are decoded using `TextDecoder`.
5. The hook parses the chunks (handling standard Server-Sent Events formats: `data: { chunk: "Hello" }`).
6. The parsed text is appended to a local React state variable representing the currently generating message.
7. React renders this state, creating the typewriter effect.

---

## 2. Structured AI Responses (Tool Calling)

Modern AI interactions go beyond raw text. The Kaevo AI agents (A1, A2) use LLM tool calling/functions to trigger specific UI components.

### The Payload Protocol
The backend LLM Gateway is instructed to return a specific JSON protocol alongside or instead of text.

```json
{
  "message": {
    "role": "assistant",
    "content": "I'd recommend our AI Voice Assistant for that.",
    "tools_invoked": [
      {
        "name": "render_service_card",
        "arguments": {
          "service_id": "ai-voice",
          "highlight": "24/7 Phone Support"
        }
      }
    ]
  }
}
```

### The Render Engine
The frontend `<MessageFeed />` acts as a compiler. When it iterates over the message array:
1. It renders the `content` string as standard Markdown text.
2. It inspects the `tools_invoked` array.
3. If it finds `render_service_card`, it dynamically imports and mounts the `<InteractiveServiceCard />` component directly below the text bubble, passing the `arguments` as props.

---

## 3. Suggestion Prompts System

To reduce friction and guide the user, the AI dictates what the user might want to say next.

### Contextual Suggestions
Instead of static buttons, the AI appends contextual suggestions to its responses.
- **Example:** After explaining pricing, the AI payload includes `suggestions: ["Book a call to discuss", "Can you customize a plan?"]`.
- The frontend maps these into `<SuggestionChip />` components placed above the chat input.

### Auto-Submit
When a user clicks a `<SuggestionChip />`, the frontend:
1. Visually removes the chips.
2. Immediately dispatches the chip's text as a new user message.
3. Triggers the streaming response cycle.

---

## 4. Markdown and Code Formatting

AI responses often include formatting.
- **Renderer:** Use a robust library like `react-markdown` to parse the `content` string safely.
- **Security:** Strict HTML sanitization (e.g., `rehype-sanitize`) is mandatory to prevent XSS attacks if the LLM hallucinates or is injected with malicious HTML.
- **Code Blocks:** Use a syntax highlighter (e.g., `react-syntax-highlighter`) to style `pre` and `code` blocks beautifully, matching the JetBrains Mono typography tokens. Include a "Copy to Clipboard" button on all code blocks.

---

## 5. Human-in-the-Loop (HITL) Interruption

If the AI confidence drops or the user requests a human, the UI must shift gracefully.
- **Status Change:** The header status changes from "Online (AI)" to "Routing to Team...".
- **Visual Distinction:** Once a human takes over, the avatar changes from the Kaevo AI logo to the human agent's profile picture, and the bubble background color shifts slightly to indicate a human is speaking.
