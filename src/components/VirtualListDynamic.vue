<template>
  <div ref="container" class="container" @scroll="scrollHandle">
    <div ref="main" class="list-main"></div>
    <div
      ref="list"
      class="list"
      :style="{ transform: `translateY(${startOffset}px)` }"
    >
      <div
        ref="items"
        class="list-item"
        v-for="item in visiableData"
        :key="item.id"
        :id="item.id"
      >
        {{ item.value }}
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    estimatedItemSize: {
      type: Number,
      default: 200,
    },
    listData: {
      type: Array,
      default: () => [],
    },
  },
  data() {
    return {
      position: [],
      visiableHeight: 0,
      start: 0,
      end: 0,
      startOffset: 0,
    };
  },
  created() {
    this.initPositions();
  },
  mounted() {
    this.visiableHeight = window.innerHeight;
    this.start = 0;
    this.end = this.start + this.visiableCount;
  },
  updated() {
    this.$nextTick(() => {
      if (!this.$refs.items || !this.$refs.items.length) return;
      // 更新缓存位置和尺寸
      this.updatePosition();
      // 更新列表容器总高度
      let height = this.position[this.position.length - 1].bottom;
      this.$refs.main.style.height = height + "px";
      this.setStartOffset();
    });
  },
  computed: {
    // 渲染列表项数量
    visiableCount() {
      return Math.ceil(this.visiableHeight / this.estimatedItemSize);
    },
    // 渲染列表项数据
    visiableData() {
      return this.listData.slice(this.start, this.end);
    },
  },
  methods: {
    // 初始化缓存
    initPositions() {
      this.position = this.listData.map((d, index) => ({
        index,
        height: this.estimatedItemSize,
        top: index * this.estimatedItemSize,
        bottom: (index + 1) * this.estimatedItemSize,
      }));
    },
    // 更新缓存位置和尺寸
    updatePosition() {
      let nodes = this.$refs.items;
      nodes.forEach((node) => {
        let rect = node.getBoundingClientRect();
        let newHeight = rect.height;
        let index = +node.id; // 通过node去拿index索引
        let oldHeight = this.position[index].height

        let diff = newHeight - oldHeight
        if (diff) {
          this.position[index].height = newHeight;
          this.position[index].bottom = this.position[index].bottom + diff;

          for (let i = index + 1; i < this.position.length; i++) {
            this.position[i].top = this.position[i - 1].bottom;
            this.position[i].bottom = this.position[i].bottom + diff;
          }
        }
      });
    },
    //二分优化：获取列表起始索引
    getStartIndex(scrollTop = 0) {
      //二分法查找
      return this.binarySearch(this.position, scrollTop);
    },
    //二分法查找
    binarySearch(q, x) {
      let l = 0, r = q.length - 1
      while(l < r) {
        let mid = Math.floor((l + r) / 2)
        if(q[mid].bottom >= x) r = mid
        else l = mid + 1
      }
      return l
    },
    // 可视窗口的偏移量
    setStartOffset() {
      this.startOffset =
        this.start >= 1 ? this.position[this.start - 1].bottom : 0;
    },
    // 监听滚动事件
    scrollHandle() {
      let scrollTop = this.$refs.container.scrollTop;
      this.start = this.getStartIndex(scrollTop)
      this.end = this.start + this.visiableCount;
      this.setStartOffset();
    },
  },
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
