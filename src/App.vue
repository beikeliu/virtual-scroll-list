<script setup>
import { onMounted, ref } from "vue";
import VirtualScroll from "./VirtualScroll.vue";
const time = ref(0);
let start = new Date();
onMounted(() => {
  time.value = new Date() - start;
});

const getData = () => {
  const data = [];
  for (let n = 0; n < 300000; n++) {
    data.push(n + 1);
  }
  return data;
};
const bind = {
  itemHeight: 25, // 每一项的高度
  listHeight: 500, // 容器高度
  data: getData(), // 数据
};
</script>

<template>
  <section>
    <header>
      本次放了30万条数据，共花费 <b>{{ time }}</b> ms
    </header>
    <VirtualScroll v-bind="bind" />
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
