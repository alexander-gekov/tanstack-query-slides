---
theme: seriph
title: AI in Vue using Vercel AI SDK + Cloudflare Workers
subtitle: Vue.js Global Summit ’25 • AI Edition
info: Integrating AI into Vue apps with minimal setup, better structure, and no server hassle
author: Alexander Gekov
---

# AI in Vue  
using Vercel AI SDK + Cloudflare Workers

**Vue.js Global Summit ’25 • AI Edition**  
**Alexander Gekov**  

---

# About Me

- Co‑founder & CTO at TalentSight (RecTech Startup)  
- 5+ years in Frontend Development  
- Host of a Vue.js & Frontend YouTube Channel  

---

# Why AI Is Integral to Modern Web Apps

- Personalized UIs & recommendations  
- Natural language interfaces (chat, search, commands)  
- Automated content generation & summarization  
- Smarter forms & validation  
- Real‑time data insights  

---

# A Naïve AI Integration Approach

```text
// You pick one provider's API or SDK
fetch('https://api.provider.com/v1/generate', {...})
```

- Works… but tightly coupled  
- Every provider uses different endpoints & parameters  
- Migrating to a new model = major rewrite  

**→** We need an abstraction layer to reduce headaches

---

# Vercel AI SDK

- Open‑source TypeScript library by Vercel  
- Unified interface over multiple LLM providers  
- Swap models & providers with minimal code changes  
- Works in Node, frontends, and Serverless  

---

# Vercel AI SDK: Three Parts

1. **Core**  
   - Low‑level primitives (generateText, streamText, etc.)  
2. **UI**  
   - React components & hooks for chats, completions  
3. **RSC**  
   - React Server Components (we'll skip this today)  

---

# Setup in Your Vue/Nuxt App

```bash
npm install ai @ai-sdk/vue zod
```

- In Nuxt: use **server routes** under `/server/api/ai`  
- In Vue you can pair with any Node/Edge backend  
- No lock‑in—swap Vercel AI for Cloudflare Workers AI easily  

---

# Core: `generateText`

```ts
import { generateText } from 'ai'

const res = await generateText({
  model: 'openai/gpt-4o',
  prompt: 'Give me a creative name for a Vue plugin',
})
console.log(res.text)
```

- Basic building block  
- Change `model` or provider with 1‑line edit  

---

# Core: `streamText`

```ts
const stream = await streamText({ ... })
for await (const chunk of stream) {
  console.log(chunk.text)
}
```

- Real‑time streaming responses  
- Hook up to your UI for live typing effects  

---

# Structured Outputs with Zod

```ts
import { generateObject } from 'ai'
import { z } from 'zod'

const schema = z.object({
  title: z.string(),
  bullets: z.array(z.string()),
})

const result = await generateObject({ schema, prompt: '...' })
console.log(result.title, result.bullets)
```

- Guarantees a JSON shape  
- Simplifies parsing & downstream UI  

---

# Tool Calling

- Pass "tools" (calculators, search, etc.) into LLM context  
- Example: `{ name: 'wikiSearch', exec: async (q) => {...} }`  
- LLM learns to invoke and format calls  

---

# AI SDK UI in Vue

- Official hooks for React only: `useChat`, `useCompletion`, etc.  
- In Vue you can wrap `generateText` & streaming primitives  
- Minimal boilerplate → build custom chat components  

---

# Chat Bot Use Case

- **Messages** array of `{ role, content }`  
- **History** persisted in state or server  
- **Streaming UI** with `<textarea>` or custom `<ChatBubble>`  

---

# NuxtHub AI

- Deployment & Edge runtime with Cloudflare Workers  
- First‑class AI support via [@nuxt/ai](https://github.com/nuxt-community/ai)  
- Zero‑cold‑start, global scale  

---

# NuxtHub Setup

```bash
npx nuxi init nuxt-ai-app
cd nuxt-ai-app
npm install @nuxt/ai
```

```js
// nuxt.config.ts
export default defineNuxtConfig({
  modules: ['@nuxt/ai'],
  ai: {
    providers: {
      vercel: { apiKey: process.env.VERCEL_AI_KEY },
      cf: { apiKey: process.env.CF_API_KEY }
    }
  }
})

---

# Conclusion

- Unified AI abstraction in Vue/Nuxt via Vercel AI SDK  
- Seamless serverless & edge deployments with NuxtHub + Cloudflare Workers  
- Benefits: minimal setup, no vendor lock-in, structured outputs  
- Get started: https://vercel.ai | https://github.com/nuxt-community/ai  
- Thank you! Questions?

