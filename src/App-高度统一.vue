<template>
  <div id="app">
    <!-- 高度统一 -->
    <VirtualList :listData="listData" :itemSize="itemSize"></VirtualList>
  </div>
</template>

<script>
import VirtualList from "./components/VirtualList.vue";
export default {
  name: "App",
  components: {
    VirtualList,
  },
  data() {
    return {
      listData: [],
      itemSize: 100,
    };
  },
  mounted() {
    for (let i = 0; i < 1000; i++) {
      this.listData.push({ id: i, value: i });
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
