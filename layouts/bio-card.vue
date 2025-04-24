<template>
  <div class="slidev-layout bio-card">
    <div class="my-auto">
      <slot name="header">
        <h1><slot></slot></h1>
      </slot>
      
      <div class="grid grid-cols-2 gap-4 mt-4">
        <div class="flex items-center justify-center">
          <slot name="image"></slot>
        </div>
        
        <div class="flex flex-col justify-center">
          <slot name="content"></slot>
        </div>
      </div>
    </div>
  </div>
</template> 