<script>
import { defineComponent } from 'vue';
import ProductViewer from '@/product-viewer.vue';

export default defineComponent({
  name: 'ServeDev',
  components: {
    ProductViewer
  },
  methods: {
    onDragRight(){
      console.log('right')
    },
    onDragLeft(){
      console.log('left')
    },
  }
});
</script>

<template>
  <div id="app">
    <div class="wrapper">
      <product-viewer 
        @drag-right="onDragRight"
        @drag-left="onDragLeft"
      >
        <template #header>Header</template>
      </product-viewer>
    </div>
  </div>
</template>

<style>
body {
  margin: 0;
}
#app {
  margin: 0;
  height: 50vh;
  width: 100vw;
}

.wrapper {
  width: 400px;
  height: 400px;
  margin: 0 auto;
}
</style>