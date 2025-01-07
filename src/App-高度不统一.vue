<template>
  <div id="app">
    <!-- 高度动态不统一 -->
    <VirtualListDynamic :listData="listData" :estimatedItemSize="estimatedSize">
      
    </VirtualListDynamic>
  </div>
</template>

<script>
import VirtualListDynamic from "./components/VirtualListDynamic.vue";
import { faker } from '@faker-js/faker';

export default {
  name: "App",
  components: {
    VirtualListDynamic
  },
  data() { 
    return {
      listData: [],
      estimatedSize: 100,
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
