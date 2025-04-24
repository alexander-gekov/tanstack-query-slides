---
theme: seriph
title: AI in Vue using Vercel AI SDK + Cloudflare Workers
subtitle: Vue.js Global Summit '25 • AI Edition
info: Integrating AI into Vue apps with minimal setup, better structure, and no server hassle
author: Alexander Gekov
---

# AI in Vue  
using Vercel AI SDK + Cloudflare Workers

**Vue.js Global Summit '25 • AI Edition**  
**Alexander Gekov**  

---
layout: bio-card
---

# About Me

::image::
<div class="flex items-center justify-center">
  <img src="./assets/images/casual.jpeg" class="rounded-lg shadow-xl w-80" />
</div>
::

::content::
<div class="space-y-4">
  <h2 class="text-2xl font-bold">Alexander Gekov</h2>
  
  <ul class="space-y-2">
    <li>🚀 Co‑founder & CTO at TalentSight (RecTech Startup)</li>
    <li>💻 5+ years in Frontend Development</li>
    <li>🎥 Vue.js & Frontend YouTube Channel</li>
    <li>🇧🇬 Based in Bulgaria</li>
    <li>🏎️ Formula 1 enthusiast</li>
    <li>🏃‍♂️ Running & 🎾 Tennis player</li>
  </ul>

  <div class="flex items-center space-x-4">
    <a href="https://twitter.com/AlexanderGekov" target="_blank" class="text-blue-500 hover:text-blue-600">
      <carbon-logo-twitter class="text-2xl" />
    </a>
    <a href="https://linkedin.com/in/alexander-gekov" target="_blank" class="text-blue-500 hover:text-blue-600">
      <carbon-logo-linkedin class="text-2xl" />
    </a>
    <a href="https://youtube.com/@AlexanderGekov" target="_blank" class="text-red-500 hover:text-red-600">
      <carbon-logo-youtube class="text-2xl" />
    </a>
    <a href="https://github.com/alexander-gekov" target="_blank" class="text-gray-500 hover:text-gray-600">
      <carbon-logo-github class="text-2xl" />
    </a>
  </div>
</div>

---

# Why AI Is Integral to Modern Web Apps

<div class="grid grid-cols-2 gap-4">
<div>

## User Expectations
- AI features are now **expected by default**
- Apps without AI feel "outdated"
- Natural interfaces (chat, voice, search)
- Personalized experiences at scale

</div>
<div>

## Developer Reality
- AI is no longer a "nice to have"
- Integration skills are **essential**
- Understanding AI capabilities is crucial
- Proper implementation matters

</div>
</div>

<div class="mt-8 space-y-4">

## Why We Need to Master AI Integration
- 🎯 Users compare every app to ChatGPT
- 🚀 Competitors are rapidly adopting AI
- 💡 AI enhances core functionality

</div>

---

# A Initial AI Integration Approach

```ts {all|1-2|4-12|14-22}
// Direct HTTP call to OpenAI
fetch('https://api.openai.com/v1/chat/completions', {
  method: 'POST',
  headers: {
    'Authorization': `Bearer ${OPENAI_API_KEY}`,
    'Content-Type': 'application/json'
  },
  body: JSON.stringify({
    model: 'gpt-4',
    messages: [{ role: 'user', content: prompt }]
  })
})

// Or using their SDK
import OpenAI from 'openai'
const openai = new OpenAI()

const response = await openai.chat.completions.create({
  model: 'gpt-4',
  messages: [{ role: 'user', content: prompt }]
})
```

<div class="mt-4 text-sm opacity-70">
❌ Tightly coupled to one provider
</div>

---

# The Problems with Naive Integration

<div class="grid grid-cols-2 gap-4">
<div>

## Implementation Issues
- Tied to specific provider
- Different endpoints per provider
- Different parameter formats
- Hard to test new models

</div>
<div>

## The Solution
- Use an abstraction layer
- Unified interface for all providers
- Easy model switching
- → Enter Vercel AI SDK

</div>
</div>

---

# Vercel AI SDK

<div class="grid grid-cols-2 gap-4">
<div>

## Core Features
- Open-source TypeScript library
- Unified interface for all LLM providers
- Simple model switching
- Built-in TypeScript types
- Framework agnostic

</div>
<div>

## Key Benefits
- No vendor lock-in
- Streaming responses
- Structured outputs
- Tool calling support
- Built-in UI components

</div>
</div>

<div class="mt-6">

```ts {all|1-2|4-7}
import { generateText } from 'ai'
import { openai } from '@ai-sdk/openai'

const { text } = await generateText({
  model: openai('o3-mini'),
  prompt: 'What is love?'
})
```

</div>

<div class="abs-br m-6 text-xs opacity-70">
Learn more: sdk.vercel.ai
</div>

---

# Vercel AI SDK: Three Parts

<div class="grid grid-cols-2 gap-8">
<div>

## 1. Core
- Provider-agnostic unified API
- Works in any JS environment
- Key functions:
  - `generateText` for text generation
  - `generateObject` for structured data
  - Tool calling capabilities

## 3. RSC (React Server Components)

</div>
<div>


## 2. UI
- Framework-agnostic composables
- Full Vue.js/Nuxt support with:
  - `useChat`
  - `useCompletion`
  - `useObject`
</div>
</div>

---

# Setup in Your Vue/Nuxt App

```bash {all|1|2-3|4|5}
npm install ai                 # Core SDK
npm install @ai-sdk/openai     # OpenAI provider
npm install @ai-sdk/vue        # Vue UI components
npm install zod               # Structured outputs
```

## What Each Package Does

<div class="grid grid-cols-2 gap-4 mt-4">
<div>

### Core Packages
- `ai`: Main SDK with core functionality (currently version 4.3)
- `@ai-sdk/openai`: OpenAI provider integration
- Can swap providers with one line of code

</div>
<div>

### UI & Validation
- `@ai-sdk/vue`: Vue-specific UI components
- `zod`: Type-safe structured outputs
- Auto-imports in Nuxt

</div>
</div>

<div class="mt-4 text-sm opacity-80">
💡 Other providers available: Anthropic, Google AI, xAI, etc. even local LLMs through Ollama
</div>  

---

# Configure OpenAI API Key

<div class="grid grid-cols-2 gap-4">
<div>

## Environment Setup
```bash
# .env
OPENAI_API_KEY=sk-xxx...
```

💡 Never commit API keys to version control

</div>
<div>

## Nuxt Configuration

```ts
// nuxt.config.ts
export default defineNuxtConfig({
  runtimeConfig: {
    openaiApiKey: process.env.OPENAI_API_KEY
  }
})
```

</div>
</div>

---

# generateText: Basic Text Generation

<div class="grid grid-cols-2 gap-4">
<div>

## Basic Usage
```ts
// server/api/generate.ts
import { generateText } from 'ai'
import { openai } from '@ai-sdk/openai'

export default defineEventHandler(async (event) => {
  const { prompt } = await readBody(event)
  
  const { text } = await generateText({
    model: openai('gpt-4o-mini'),
    system: 'You are a helpful assistant.',
    prompt
  })
  
  return text
})
```

</div>
<div>

## Easy Model Switching
```ts
import { anthropic } from '@ai-sdk/anthropic'
import { google } from '@ai-sdk/google'

// Switch to Claude
const { text } = await generateText({
  model: anthropic('claude-3.5-sonnet'),
  prompt
})

// Or Google Gemini
const { text } = await generateText({
  model: google('gemini-2.5-flash'),
  prompt
})
```

</div>
</div>

<div class="mt-4 text-sm opacity-80">
💡 One interface, multiple providers - just swap the model parameter
</div>

---

# streamText: Real-time Streaming Responses

<div class="grid grid-cols-2 gap-4">
<div>

## Server Implementation
```ts
// server/api/stream.ts
import { streamText } from 'ai'
import { openai } from '@ai-sdk/openai'

export default defineEventHandler(async (event) => {
  const { prompt } = await readBody(event)

  const result = streamText({
    model: openai('gpt-4o-mini'),
    prompt
  })

  // Debug: See chunks in console
  for await (const chunk of result.textStream) {
    process.stdout.write(chunk)
  }

  return result.toDataStreamResponse()
})
```

</div>
<div>

## Benefits of Streaming
- ⚡️ Instant first response
- 🔄 Progressive content display
- 🎯 Better UX - no loading states
- 📱 Feels more interactive
- ⏱️ Eliminates waiting for full response

<div class="mt-4">
```ts
// Console output example:
"I" → "I think" → "I think that" → 
"I think that streaming" → 
"I think that streaming is" →
"I think that streaming is awesome!"
```
</div>

</div>
</div>

<div class="mt-4 text-sm opacity-80">
💡 Returns DataStreamResponse for seamless integration with UI components
</div>

---

# generateObject: Structured Outputs

<div class="grid grid-cols-2 gap-4">
<div>

## Type-Safe AI Responses
```ts
// server/api/structured.ts
import { generateObject } from 'ai'
import { openai } from '@ai-sdk/openai'
import { z } from 'zod'

export default defineEventHandler(async (event) => {
  const { prompt } = await readBody(event)

  const { object } = await generateObject({
    model: openai('gpt-4o-mini'),
    prompt,
    schema: z.object({
      city: z.string(),
      funFact: z.string(),
      country: z.string(),
      population: z.number(),
    })
  })

  return object // ✨ Type-safe!
})
```

</div>
<div>

## Real-World Use Cases

### 🏢 Recruitment Platform
```ts
const schema = z.object({
  hasStartupExperience: z.boolean(),
  yearsOfLeadership: z.number(),
  techStack: z.array(z.string()),
  cultureFit: z.number().min(1).max(10)
})
```

### 📊 Content Analysis
```ts
const schema = z.object({
  sentiment: z.enum(['positive', 'negative']),
  toxicity: z.number().min(0).max(1),
  keywords: z.array(z.string())
})
```

</div>
</div>

<div class="mt-4 text-sm opacity-80">
💡 Force AI to return exactly the data structure you need, with type safety!
</div>

---

# streamObject: Real-time Structured Data

<div class="grid grid-cols-2 gap-4">
<div>

## Streaming Objects
```ts
// server/api/stream-structured.ts

export default defineEventHandler(async (event) => {
  const { prompt } = await readBody(event)

  const response = streamObject({
    model: openai('gpt-4o-mini'),
    prompt,
    schemaName: 'City',
    schemaDescription: 
      'A city with a name, fun fact, country, and population.',
    schema: z.object({
      city: z.string(),
      funFact: z.string(),
      ...
    })
  })

  return response.toTextStreamResponse()
})
```

</div>
<div>

### Benefits
- 🔄 Progressive object construction
- ⚡️ Instant UI feedback
- 🎯 Type-safe at every step
- 🛡️ Schema validation while streaming

</div>
</div>

---

# Returns

<div class="grid grid-cols-2 gap-4">
<div>

## Implementation
```ts
// server/api/metadata.ts
export default defineEventHandler(async (event) => {
  const { prompt } = await readBody(event)

  const {
    text,
    sources,      // Reference sources used
    reasoning,    // Model's thought process
    toolCalls,    // Tools used during generation
    usage,        // Token usage statistics
    finishReason  // Why generation stopped
  } = await generateText({
    model: openai('gpt-4o-mini'),
    prompt
  })

  return { text, sources, reasoning, 
    toolCalls, usage, finishReason }
})
```

</div>
<div>

## We can get a lot of information from the response

### 🔍 Sources
- Reference materials used
- Citations and links
- Search results used

### 🧠 Reasoning
- Step-by-step thought process
- Available with models like:
  - `gpt-o3`
  - `claude-3.7-sonnet`

</div>
</div>

---

# Returns

<div class="grid grid-cols-3 gap-4">
<div>

### 📊 Usage Stats
- Prompt Tokens
- Completion Tokens

</div>
<div>

### 🛠️ Tool Calls
- Tool calls made

</div>
<div>

### 📝 Object
- When using `generateObject` or `streamObject`

</div>
<div>

### ✅ Finish Reason
- Reason for stopping generation

</div>
</div>

---

# Chat History & Message Prompts

<div class="grid grid-cols-2 gap-4">
<div>

## Maintaining Context
- Instead of a single `prompt`, use an array of `messages`
- Each message has `role` (`user`, `assistant`, `tool`) and `content`
- Ideal for chat interfaces and complex interactions

```ts
// Example messages array
const messages = [
  { role: 'user', content: 'Hi!' },
  { role: 'assistant', content: 'Hello! How can I help?' },
  { role: 'user', content: 'Where is the best Currywurst?' }
]
```

<div class="mt-4 text-sm opacity-80">
💡 Content can also be an array of parts (text, images, etc.) for multi-modal inputs.
</div>

</div>
<div>

## Backend Implementation
```ts
// server/api/chat.ts
import { openai } from '@ai-sdk/openai'
import { streamText } from 'ai'

export default defineEventHandler(async (event) => {
  const { messages } = await readBody(event)

  const result = streamText({
    model: openai('gpt-4o-mini'),
    system: 'You are a helpful assistant.',
    messages // Pass the whole history
  })

  return result.toDataStreamResponse()
})
```

</div>
</div>

---

# Frontend: useChat Composable

<div class="grid grid-cols-2 gap-8">
<div>

## `@ai-sdk/vue` 

```vue
<script setup lang="ts">
const { messages, input, handleSubmit } = useChat()
</script>

<template>
  <div>
    <div v-for="m in messages" :key="m.id">
      {{ m.role === 'user' ? 'User: ' : 'AI: ' }}
      {{ m.content }}
    </div>

    <form @submit="handleSubmit">
      <input v-model="input" placeholder="Say something..." />
      <button type="submit">Send</button>
    </form>
  </div>
</template>
```

</div>
<div>

## Key Features & Options

### `initialMessages`
- Start chat with existing messages:
```ts
const { messages } = useChat({
  initialMessages: [
    { id: '1', role: 'assistant', content: 'Welcome!' }
  ]
})
```

### `api` endpoint
- Specify the backend endpoint:
```ts
const { messages } = useChat({ api: '/api/chat' })
```
</div>
</div>

<div class="mt-4 text-sm opacity-80">
💡 `useChat` simplifies building interactive chat UIs in Vue/Nuxt.
</div>

---

# Tool Calling: Backend Implementation

<div class="grid grid-cols-2 gap-4">
<div>

## Extending AI with Tools
- Allow the LLM to call external functions or APIs
- Provide tools for tasks the AI can't do alone (e.g., fetch data, calculations)

## Defining a Tool (`tool()`)
- `description`: Helps AI choose the tool
- `parameters`: Zod schema for inputs
- `execute`: Async function with tool logic

</div>
<div>

</div>
</div>

---

## Example: Server API
```ts {10-18|19-23}
// server/api/tools.ts
import { streamText, tool } from 'ai'
import { openai } from '@ai-sdk/openai'
import { z } from 'zod'

export default defineEventHandler(async (event) => {
  const { messages } = await readBody(event)
  const result = streamText({
    model: openai('gpt-4o-mini'),
    messages,
    tools: {
      joke: tool({
        description: 'Get a random joke',
        parameters: z.object({}),
        execute: async () => {
          const r = await $fetch('https://icanhazdadjoke.com/', { headers: { Accept: 'application/json' } })
          return r.joke
        }
      }),
      quickMaths: tool({
        description: 'Add two numbers',
        parameters: z.object({ a: z.number(), b: z.number() }),
        execute: async ({ a, b }) => a + b
      })
    }
  })
  return result.toDataStreamResponse()
})
```

---

# Tool Calling: Frontend Rendering

## Vue Component Example
```vue {all|1-2|4-5}
<script setup lang="ts">
import { useChat } from '@ai-sdk/vue'

// Connect to the backend endpoint where tools are defined
const { messages, input, handleSubmit } = useChat({ api: '/api/tools' })
</script>
```

---

```vue {all|3|4|5|6|7-14|19-23}
<template>
  <div>
    <div v-for="m in messages" :key="m.id" class="message">
      <strong>{{ m.role }}:</strong>
      <div v-for="(part, i) in m.parts" :key="i" class="part">
        <span v-if="part.type === 'text'">{{ part.text }}</span>
        <div v-else-if="part.type === 'tool-invocation'" class="tool-call">
          Calling tool: <strong>{{ part.toolName }}</strong> 
          with args: <code>{{ JSON.stringify(part.args) }}</code>
        </div>
        <div v-else-if="part.type === 'tool-result'" class="tool-result">
          Tool <strong>{{ part.toolName }}</strong> result: 
          <code>{{ JSON.stringify(part.result) }}</code>
        </div>
      </div>
    </div>
    
    <!-- Input Form -->
    <form @submit="handleSubmit">
      <input v-model="input" placeholder="Ask for a joke or calculation..." />
      <button type="submit">Send</button>
    </form>
  </div>
</template>
```

---

# NuxtHub: Deploy on Cloudflare with Ease

<div class="grid grid-cols-2 gap-4">
<div>

## Vercel AI SDK Everywhere
- Open-source & framework-agnostic
- Works in any JS environment (Node, Edge, Workers)
- Seamless integration with Cloudflare Workers

</div>
<div>

## NuxtHub Hosting
- Cloudflare-powered global edge platform
- 1-command deployment: `npx nuxthub deploy`
- Built-in SQL, KV, Blob & AI/Vectorize helpers
- We can use Cloudflare Workers AI with Vercel AI SDK

</div>
</div>

---

# NuxtHub AI: Feature Overview

- Integrate ML models into Nuxt via Cloudflare Workers AI  
- Supports text, image, embeddings, and more  
- Uses `hubAI()` server composable for a simple API

---

# Install NuxtHub

```shell
npx nuxi module add hub
```

```shell
npm install --save-dev wrangler
```

---

# Enabling AI in NuxtHub

```ts
// nuxt.config.ts
export default defineNuxtConfig({
  hub: { ai: true }
})
```

---

# Running Text Models with hubAI()

```ts
// server/api/ai-text.ts
export default defineEventHandler(async () => {
  const ai = hubAI()
  return await ai.run('@cf/meta/llama-3.1-8b-instruct', {
    prompt: 'Who is the author of Nuxt?'
  })
})
```

---

# Image Generation with hubAI()

```ts
// server/api/image.ts
export default defineEventHandler(async () => {
  return await hubAI().run(
    '@cf/runwayml/stable-diffusion-v1-5-img2img',
    { prompt: 'A sunset over the ocean.' }
  )
})
```

---

# Generating Embeddings

```ts
// server/api/embeddings.ts
export default defineEventHandler(async () => {
  return await hubAI().run(
    '@cf/baai/bge-base-en-v1.5',
    { text: 'NuxtHub AI uses `hubAI()` to run models.' }
  )
})
```

---

# Defining Tools for Workers AI

```ts
// server/api/tools.ts
const tools = [
  {
    name: 'get-weather',
    description: 'Gets the weather for a given city',
    parameters: { type: 'object', properties: { city: { type: 'string' } }, required: ['city'] },
    function: async ({ city }) => {
      // fetch weather
      return '72°F'
    }
  }
]
```

---

# Streaming Text with hubAI()

```ts
// server/api/stream.ts
export default defineEventHandler(async () => {
  const stream = await hubAI().run(
    '@cf/meta/llama-3.1-8b-instruct',
    {
      prompt: 'Hello AI!',
      stream: true,
      messages: [
        { role: 'system', content: 'You are a friendly assistant' },
        { role: 'user', content: 'How are you?' }
      ]
    }
  )
  return stream
})
```

---

# Handling Streaming Responses on Client

```ts
// client stream handler
const response = await $fetch<ReadableStream>('/api/stream', {
  method: 'POST',
  body: { query: 'Hello AI!' },
  responseType: 'stream'
})
const reader = response
  .pipeThrough(new TextDecoderStream())
  .getReader()
while (true) {
  const { value, done } = await reader.read()
  if (done) break
  console.log('Received:', value)
}
```

---

# Vercel AI SDK with Workers AI

```ts
// server/api/chat.post.ts
import { streamText } from 'ai'
import { createWorkersAI } from 'workers-ai-provider'

export default defineEventHandler(async (event) => {
  const { messages } = await readBody(event)
  const workersAI = createWorkersAI({ binding: hubAI() })
  return streamText({
    model: workersAI('@cf/meta/llama-3.1-8b-instruct'),
    messages
  }).toDataStreamResponse()
})
```

---

# Full Chat Component Example

```vue {all}
<script setup lang="ts">
import { useChat } from '@ai-sdk/vue'

const { messages, input, handleSubmit, isLoading, stop, error, reload } = useChat()
</script>

<template>
  <div v-for="m in messages" :key="m.id">
    {{ m.role }}: {{ m.content }}
  </div>
  <div v-if="error">
    <div>{{ error.message || 'An error occurred' }}</div>
    <button @click="reload">retry</button>
  </div>
  <form @submit="handleSubmit">
    <input v-model="input" placeholder="Type here..." />
    <button v-if="isLoading" @click="stop">stop</button>
    <button v-else type="submit">send</button>
  </form>
</template>
```

---

# Conclusion

- Unified AI abstraction in Vue/Nuxt via Vercel AI SDK  
- Seamless serverless & edge deployments with NuxtHub + Cloudflare Workers  
- Benefits: minimal setup, no vendor lock-in, structured outputs  
- Get started: https://vercel.ai | https://github.com/nuxt-community/ai  
- Thank you! Questions?

