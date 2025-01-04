<template>
  <div ref="list" class="container" @scroll="scrollEvent($event)">
    <div class="list-main" :style="{ height: listHeight + 'px' }"></div>
    <div
      class="list-visible"
      ref="visibleRef"
      :style="{ transform: `translateY(${startOffset}px)` }"
    >
      <div
        class="list-item"
        v-for="item in visibleData"
        :key="item.id"
        :style="{ height: itemSize + 'px' }"
      >
        {{ item.value }}
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "VirtualList",
  props: {
    listData: {
      type: Array,
      default: () => [],
    },
    itemSize: {
      type: Number,
      default: 200,
    },
  },
  data() {
    return {
      start: 0,
      end: 0,
      visibleHeight: 0,
      startOffset: 0,
    };
  },
  computed: {
    listHeight() {
      return this.listData.length * this.itemSize;
    },
    visibleCount() {
      return Math.ceil(this.visibleHeight / this.itemSize);
    },
    visibleData() {
      return this.listData.slice(
        this.start,
        Math.min(this.end, this.listData.length)
      );
    },
  },
  mounted() {
    this.visibleHeight = window.innerHeight;
    this.start = 0;
    this.end = this.start + this.visibleCount;
  },
  methods: {
    scrollEvent() {
      let scrollTop = this.$refs.list.scrollTop;
      this.start = Math.floor(scrollTop / this.itemSize);
      // this.startOffset = scrollTop - (scrollTop % this.itemSize);
      this.startOffset = this.itemSize * this.start
      this.end = this.start + this.visibleCount;
    },
  },
};
</script>

<style>
.container {
  width: 100%;
  height: 100%;
  /* 让改容器可以滚动，显示滚动条 */
  overflow-y: auto;
  position: relative;
  /* 测试是否出现滚动条 */
  margin-top: 200px;
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
