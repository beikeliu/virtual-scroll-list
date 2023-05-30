<script setup>
import { ref, onMounted } from "vue";
import debounce from "lodash.debounce";
// 数据定义
const count = ref(100000);
const itemHeight = ref(25);
const listHeight = ref(500);
const translateY = ref(0);
const container = ref(null);
const renderItems = ref([]);
const array = [];

init();
onMounted(() => {
  container.value.addEventListener("scroll", debounce(onScroll, 25));
});

function init() {
  // 初始化数据
  for (let n = 0; n < count.value; n++) {
    array.push(n + 1);
  }
  // 初始渲染0 到 25 行
  renderItems.value = array.slice(0, 25);
}

// 滚动条改变要干的事情
function onScroll(e) {
  const { scrollTop } = e.target;
  // 滚动过的位置搞成空白填充
  translateY.value = scrollTop;
  // 计算开始行数： 滚动高度除以每行的高度就等于从第几行开始
  const start = Number.parseInt(scrollTop / itemHeight.value);
  // 滚动后应渲染计算出的开始行数 到 开始行数+25行
  renderItems.value = array.slice(start, start + 25);
}
</script>

<template>
  <div class="container" ref="container">
    <ol class="list">
      <li class="item" v-for="item in renderItems" :key="item">
        我是第{{ item }}行
      </li>
    </ol>
  </div>
</template>

<style scoped>
.container {
  border: 1px solid black;
  height: v-bind(listHeight + "px");
  width: 250px;
  overflow: auto;
}
.list {
  /* 添加最大高度，将滚动条视觉拉高 */
  min-height: calc(v-bind(itemHeight + "px") * v-bind(count));
}
.item {
  height: v-bind(itemHeight + "px");
  transform: translateY(v-bind(translateY + "px"));
}
</style>
