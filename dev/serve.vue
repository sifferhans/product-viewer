<script setup>
import { onBeforeMount, ref } from 'vue';
import ProductViewer from '@/product-viewer.vue';

const directionRef = ref()
const frameRef = ref()
const images = ref([])

function onDragRight({ currentFrame, totalFrames, direction }){
  frameRef.value.innerText = currentFrame + ' / ' + totalFrames
  directionRef.value.innerText = direction
}

function onDragLeft({ currentFrame, totalFrames, direction }){
  frameRef.value.innerText = currentFrame + ' / ' + totalFrames
  directionRef.value.innerText = direction
}

onBeforeMount(() => {
  for(let i = 1; i <= 34; i++) {
    images.value.push(`https://spritespin.ginie.eu/images/rad_zoom_${i<10?'00':'0'}${i}.jpg`)
  }
})
</script>

<template>
  <div id="app">
    <div class="wrapper">
      <product-viewer 
        @drag-right="onDragRight"
        @drag-left="onDragLeft"
        :images="images"
        :speed="10"
      >
        <template #header="{ activeFrame }">
          <div class="product-title">
            <b>From scoped slot</b>
            <div>Active frame: <span>{{ activeFrame }}</span></div>
          </div>
        </template>
      </product-viewer>
      <b>From events:</b>
      <div>Active frame: <span ref="frameRef"></span></div>
      <div>Delta: <span ref="directionRef"></span></div>
    </div>
  </div>
</template>

<style>
* {
  box-sizing: border-box;
}

body {
  margin: 0;
  font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
}

#app {
  padding: 2rem;
  margin: 0;
  height: 50vh;
  width: 100vw;
}

.wrapper {
  width: 480px;
  height: 327px;
  margin: 0 auto;
}

.product-title {
  padding: 0.75rem 1rem;
}
</style>