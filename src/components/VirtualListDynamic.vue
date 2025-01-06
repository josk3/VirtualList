<template>
  <div class="container">
    <div class="list-main"></div>
    <div class="list"></div>
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
