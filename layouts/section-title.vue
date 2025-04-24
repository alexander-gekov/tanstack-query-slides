<template>
  <div class="slidev-layout section-title">
    <div class="my-auto">
      <slot name="title">
        <h1 class="text-4xl font-bold mb-4"></h1>
      </slot>
      
      <slot name="subtitle">
        <h2 class="text-2xl text-gray-400"></h2>
      </slot>
    </div>
  </div>
</template>

<style>
.section-title {
  @apply text-center;
}
</style> 