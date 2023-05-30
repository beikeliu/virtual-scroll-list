<script setup>
import { ref, onMounted, computed } from "vue";
import debounce from "lodash.debounce";

const props = defineProps({
  itemHeight: Number,
  listHeight: Number,
  data: Array,
});

// 数据定义
const translateY = ref(0);
const container = ref(null);
const renderItems = ref([]);

renderItems.value = props.data.slice(0, 25);
onMounted(() => {
  container.value.addEventListener("scroll", debounce(onScroll, 25));
});

// 滚动条改变要干的事情
function onScroll(e) {
  const { scrollTop } = e.target;
  // 滚动过的位置搞成空白填充
  translateY.value = scrollTop;
  // 计算开始行数： 滚动高度除以每行的高度就等于从第几行开始
  const start = Number.parseInt(scrollTop / props.itemHeight);
  // 滚动后应渲染计算出的开始行数 到 开始行数+25行
  renderItems.value = props.data.slice(start, start + 25);
}

const containerStyle = computed(() => ({
  border: "1px solid black",
  height: `${props.listHeight}px`,
  width: "250px",
  overflow: "auto",
}));
const listStyle = computed(() => ({
  // 计算出总列表的最小高度，撑开滚动条，模拟正常数据下应有的滚动条样式
  minHeight: `${props.itemHeight * props.data.length}px`,
}));
const itemStyle = computed(() => ({
  height: `${props.itemHeight}px`,
  // 模拟已经滚过去大量数据，将滚动条滚到下面
  transform: `translateY(${translateY.value}px)`,
}));
</script>

<template>
  <div ref="container" :style="containerStyle">
    <ol :style="listStyle">
      <li :style="itemStyle" v-for="item in renderItems" :key="item">
        我是第{{ item }}行
      </li>
    </ol>
  </div>
</template>
