<template>
  <div class="slidev-layout feature-comparison">
    <div class="grid grid-cols-2 gap-8 h-full">
      <div class="flex flex-col">
        <slot name="left-title">
          <h2 class="text-2xl font-bold mb-4"></h2>
        </slot>
        <slot name="left"></slot>
      </div>
      
      <div class="flex flex-col">
        <slot name="right-title">
          <h2 class="text-2xl font-bold mb-4"></h2>
        </slot>
        <slot name="right"></slot>
      </div>
    </div>
  </div>
</template>

<style>
.feature-comparison {
  @apply p-8;
}
</style> 