---
theme: seriph
title: TanStack Query - Is this the best paradigm for Data Fetching?
subtitle: Vue.js Global Summit '25 • Data Fetching Edition
info: Exploring TanStack Query as an alternative to useFetch, $fetch & useAsyncData
author: Alexander Gekov
---

# TanStack Query

Is this the best paradigm for Data Fetching?

**Vue.js Global Summit '25 • Vue.js Day**  
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

# What is TanStack Query?

- A missing data-fetching library for server state in Vue
- Handles fetching, caching, background updates, retries, pagination, and more
- Declarative APIs: `useQuery`, `useMutation`, `useInfiniteQuery`
- Zero-config defaults, fully customizable

<div class="flex items-center justify-center mt-20">
  <TanStackLogo class="w-40" />
</div>


---

# Some History

Tanstack Query was also previously known as React Query.

But now it has grown to support other frameworks like: Angular, Svelte, Solid and Vue!

It now has 45K Stars on GitHub and the Vue version has 835K Monthly Downloads on npm.

---

# Why TanStack Query?

Before we go on, we should understand why we need this library in the first place.

---

# What is the problem?

TanStack Query is often said to be a data-fetching library, but data fetching is not the hard part!

---

# Data Fetching is the easy part

In Nuxt we have the trio of `useFetch`, `useAsyncData` and `$fetch` to help us with that.

They all do a great job and have their own little differences and use cases that justify their existence. If you are interested Alex Lichter has a great video on this topic.

---

# What about client state?

What is client state?

When we say client state we mean application state - state that is not persisted in the database. It is synchronous and can be accessed directly by the UI. Think of form values, modal open/close state, color mode preferences, etc.

---

# Client State Management

We have a lot of tools to help us manage client state.

We store state using refs and reactive objects, which then grow into composables encapsulating logic and related state and proceed to full blown state management libraries like Pinia which has stores. Like userStore, themeStore, etc.

Pinia is great and has a lot of features, but it is not the best when it comes to managing server state.

---

# Once again, server state != client state

Client state is client-owned and we can be sure it's up to date.
Client state can only be changed by us.
It does not persist when we close the browser.
It lives in the client and is instantly available (synchronous).

---

# Server State

Server state lives on the server and what we fetch can be different from what's actually on the server at that time.

It is owned by many users as they can make changes to it.

It is persisted across browser sessions.

It is asynchronous by nature and we need to wait for the Promise to resolve.

---

# So what are the options?

Before we go on with TanStack Query, I want to give a shoutout to:

- Pinia Colada - a library from Eduardo, the creator of Pinia

It promises to be the missing data fetching library for Pinia and handle the complexity of managing server state.

It has a lot of potential, but is still in beta as far as I know. Nevertheless, it is worth checking out.

---

# TanStack Query

Let's setup TanStack Query and see how it works.

## Installation

```bash
npm install @tanstack/vue-query
# or yarn add @tanstack/vue-query
```

If we are using Vue, we can just add it as a plugin:

```ts
import { VueQueryPlugin } from '@tanstack/vue-query'

app.use(VueQueryPlugin)

```

---

## Installation in Nuxt

In Nuxt we register a new Nuxt plugin.

```ts{1|5|7-9|11|13-17|19-22}
// plugins/vue-query.ts
import type { DehydratedState, VueQueryPluginOptions } from '@tanstack/vue-query'
import { VueQueryPlugin, QueryClient, hydrate, dehydrate } from '@tanstack/vue-query'

export default defineNuxtPlugin((nuxt) => {
  const vueQueryState = useState<DehydratedState | null>('vue-query')
  const queryClient = new QueryClient({
    defaultOptions: { queries: { staleTime: 5000 } },
  })
  const options: VueQueryPluginOptions = { queryClient }
  nuxt.vueApp.use(VueQueryPlugin, options)

  if (import.meta.server) {
    nuxt.hooks.hook('app:rendered', () => {
      vueQueryState.value = dehydrate(queryClient)
    })
  }

  if (import.meta.client) {
    hydrate(queryClient, vueQueryState.value)
  }
})
```

---

And then register the plugin in the `nuxt.config.ts` file:

```ts
export default defineNuxtConfig({
  plugins: ["~/plugins/vue-query.ts"],
});
```

---

# Unofficial Nuxt Module

There is an unofficial Nuxt module for TanStack Query by @peterbud.

It's also worth checking out. Now let's see how we can use TanStack Query once we have it installed.

---

# Quick Start

Most basic usage.

```vue
<script setup lang="ts">
const { data, isLoading, isError, error } = useQuery({
  queryKey: ["todos"],
  queryFn: getTodos, // the function definition, not the function call
});
</script>
<template>
  <div v-if="isLoading">Loading...</div>
  <div v-else>{{ data }}</div>
</template>
```

---

# Demo Time

Let's see how we can use TanStack Query in practice.

---

# Conclusion

- TanStack Query provides a declarative, powerful paradigm for managing server state in Vue/Nuxt.
- Features: caching, background updates, retries, pagination, invalidations, optimistic updates.
- Can reduce boilerplate and the need for additional state stores like Pinia.
- Learn more: https://tanstack.com/query/latest/docs/framework/vue/overview
