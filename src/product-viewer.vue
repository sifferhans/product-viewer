<script setup>
import { ref, defineEmits, onMounted, onUnmounted } from 'vue'

const emit = defineEmits(['drag-right','drag-left'])
const props = defineProps({
  images: Array,
  speed: Number
})

const activeFrame = ref(1)
const isDragging = ref(false)

function setIsDragging(state) {
  isDragging.value = state
}

function onDrag(event) {
  if(!isDragging.value) return
  
  if(event.movementX > 0) {
    activeFrame.value --
    if(activeFrame.value < 1) activeFrame.value = props.images.length

    emit('drag-right', {
      currentFrame: activeFrame.value,
      totalFrames: props.images.length,
      direction: event.movementX
    })
  }

  if(event.movementX < 0) {
    activeFrame.value ++
    if(activeFrame.value > props.images.length) activeFrame.value = 1

    emit('drag-left', {
      currentFrame: activeFrame.value,
      totalFrames: props.images.length,
      direction: event.movementX
    })
  }
}

onMounted(() => {
  document.addEventListener('mousemove', onDrag)
  document.addEventListener('mouseup', () => {
    if(!isDragging.value) return
    setIsDragging(false)
  })
})

onUnmounted(() => {
  document.removeEventListener('mousemove', onDrag)
  document.removeEventListener('mouseup', () => {
    if(!isDragging.value) return
    setIsDragging(false)
  })
})
</script>

<template>
  <div
    class="product-viewer"
    @mousedown="setIsDragging(true)"
  >
    <header class="product-viewer__header" v-if="$slots.header">
      <slot name="header" :active-frame="activeFrame" />
    </header>
    <div class="product-viewer__images">
      <img
        class="product-viewer__image"
        v-for="(image, i) in images"
        :key="`product-viewer-image-${i+1}`"
        :src="image"
        alt=""
        :class="{ 'product-viewer__image--active': activeFrame == i+1 }"
      />
    </div>
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
  font: inherit;
}

.product-viewer__images {
  height: 100%;
  width: 100%;
}

.product-viewer__image {
  height: 100%;
  width: 100%;
  object-fit: contain;
  display: none;
  pointer-events: none;
  user-select: none;
}

.product-viewer__image--active {
  display: block;
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