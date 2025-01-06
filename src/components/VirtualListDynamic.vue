<template>
  <div class="container">
    <div class="list-main"></div>
    <div class="list">
      <div class="list-item" v-for="item in listData" :key="item.id">
        {{ item.value }}
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    estimatedSize: {
      type: Number,
      default: 200,
    },
    listData: {
      type: Array,
      default: () => []
    }
  },
  data() {
    return {
      position: []
    }
  },
  mounted() {

  },
  methods: {
    // 初始化缓存
    initPosition() {

    }
  }
};
</script>

<style>
.container {
  width: 40%;
  height: 100%;
  /* 让改容器可以滚动，显示滚动条 */
  overflow-y: auto;
  position: relative;
  /* 测试是否出现滚动条 */
  /* margin-top: 200px; */
}

.list-main {
  width: 100%;
  position: absolute;
  z-index: -1;
}

.list-visible {
  width: 100%;
  position: absolute;
}

.list-item {
  display: flex;
  justify-content: center;
  align-items: center;
  width: 100%;
  box-sizing: border-box;
  border-bottom: 1px solid black;
}
</style>
