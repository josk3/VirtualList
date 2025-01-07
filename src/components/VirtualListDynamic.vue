<template>
  <div ref="container" class="container">
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
      positions: [],
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
    // this.$nextTick(() => {
    //   if (!this.$refs.items || !this.$refs.items.length) return;
    //   // 更新缓存位置和尺寸
    //   this.updatePosition();
    //   // 更新列表容器总高度
    //   console.log('this.position', this.position.length);
    //   let height = this.position[this.position.length - 1].bottom;
    //   this.$refs.main.style.height = height + "px";
    //   this.setStartOffset();
    // });
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
  // watch: {
  //   listData(newVal) {
  //     if (newVal.length) {
  //       this.initPositions();
  //     }
  //   },
  // },
  methods: {
    // 初始化缓存
    initPositions() {
      this.position = this.listData.map((d, index) => ({
        index,
        height: this.estimatedItemSize,
        top: index * this.estimatedItemSize,
        bottom: (index + 1) * this.estimatedItemSize,
      }));
      console.log("this.position", this.position.length);
    },
    // updatePosition() {
    //   let nodes = this.$refs.items;
    //   nodes.forEach((node) => {
    //     let rect = node.getBoundingClientRect();
    //     let newHeight = rect.height;
    //     let oldHeight = this.position[index].height
    //     let index = +node.id.slice(); // 通过node去拿index索引
    //     console.log(typeof index);

    //     console.log('index', index);

    //     let diff = newHeight - oldHeight
    //     if (diff) {
    //       this.position[index].height = newHeight;
    //       this.position[index].bottom = this.position[index].height + diff;

    //       for (let i = index + 1; i < this.position.length; i++) {
    //         this.position[i].top = this.position[i - 1].bottom;
    //         this.position[i].bottom = this.position[i].bottom + diff;
    //       }
    //     }
    //   });
    // },
    //获取列表项的当前尺寸
    // updatePosition() {
    //   let nodes = this.$refs.items;
    //   nodes.forEach((node) => {
    //     console.log("node", node);

    //     let rect = node.getBoundingClientRect();
    //     let height = rect.height;
    //     let index = +node.id.slice(0);
    //     console.log(typeof index);

    //     console.log("index", index);
    //     let oldHeight = this.positions[index].height;
    //     let dValue = oldHeight - height;
    //     //存在差值
    //     if (dValue) {
    //       this.positions[index].bottom = this.positions[index].bottom - dValue;
    //       this.positions[index].height = height;

    //       for (let k = index + 1; k < this.positions.length; k++) {
    //         this.positions[k].top = this.positions[k - 1].bottom;
    //         this.positions[k].bottom = this.positions[k].bottom - dValue;
    //       }
    //     }
    //   });
    // },
    // getStart(scrollTop) {

    // },
    // setStartOffset() {
    //   this.startOffset =
    //     this.start >= 1 ? this.position[this.start - 1].bottom : 0;
    // },
    // scrollHandle() {
    //   let scrollTop = this.$refs.container.scrollTop;
    //   let item = this.position.find((item) => item && item.bottom > scrollTop);
    //   this.start = item.index;
    //   this.end = this.start + this.visiableCount;
    //   this.setStartOffset();
    // },
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
