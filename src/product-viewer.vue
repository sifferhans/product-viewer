<script setup>
import { ref, defineEmits } from 'vue'

const emit = defineEmits(['drag-right','drag-left'])

// drag handling
const isDragging = ref(false)

function setIsDragging(state) {
  isDragging.value = state
}

function onDrag(event) {
  if(!isDragging.value) return
  
  if(event.movementX > 0) {
    emit('drag-right', event.movementX)
  }

  if(event.movementX < 0) {
    emit('drag-left', event.movementX)
  }
}
</script>

<template>
  <div
    class="product-viewer"
    @mousedown="setIsDragging(true)"
    @mouseup="setIsDragging(false)"
    @mousemove="onDrag"
  >
    <header class="product-viewer__header" v-if="$slots.header">
      <slot name="header" />
    </header>
    <canvas
      ref="productViewer"
      class="product-viewer__canvas"
    >
      <slot name="js-disabled">This component does not function properly without javascript</slot>
    </canvas>
    <footer class="product-viewer__footer" v-if="$slots.footer">
      <slot name="footer" />
    </footer>
  </div>
</template>

<style>
.product-viewer {
  position: relative;
  height: 100%;
  width: 100%;
  background: #eaeaea;
}

.product-viewer__canvas {
  height: 100%;
  width: 100%;
}

.product-viewer__header,
.product-viewer__footer {
  position: absolute;
  left: 0;
  right: 0;
}

.product-viewer__header {
  top: 0;
}

.product-viewer__footer {
  bottom: 0;
}
</style>