<template>
  <div id="app">
    <!-- 处理快速滚动白屏 -->
    <VirtualListBuffer
      :listData="listData"
      :estimatedItemSize="estimatedItemSize"
    >
    </VirtualListBuffer>
  </div>
</template>

<script>
import VirtualListBuffer from "./components/VirtualListBuffer.vue";
import { faker } from "@faker-js/faker";

export default {
  name: "App",
  components: {
    VirtualListBuffer,
  },
  data() {
    return {
      listData: [],
      estimatedItemSize: 100,
    };
  },
  // 注意：需要在created中处理数据，如果在mounted处理，listData传给子组件可能为空
  created() {
    for (let i = 0; i < 1000; i++) {
      this.listData.push({ id: i, value: faker.lorem.sentences() });
    }
  },
};
</script>

<style>
body {
  height: 100%;
  margin: 0;
}
#app {
  height: 100%;
}
html {
  height: 100%;
}
</style>
