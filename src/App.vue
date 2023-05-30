<script setup>
import { onMounted, ref } from "vue";
import VirtualScroll from "./VirtualScroll.vue";
const time = ref(0);
let start = new Date();
onMounted(() => {
  time.value = new Date() - start;
});
</script>

<template>
  <section>
    <header>本次渲染花费 <b>{{ time }}</b> ms</header>
    <VirtualScroll />
  </section>
</template>

<style>
#app {
  position: absolute;
  top: 200px;
  left: 200px;
}
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
</style>
