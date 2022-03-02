<template>
  <view class="l-falls" :style="{ height: maxHeight + 'px' }" v-if="items.length">
    <item
      @renderBox="renderBox"
      @itemClick="itemClick(s, i)"
      class="item"
      :style="
        'left:' +
        (s.left || 0) +
        'px;top:' +
        (s.top || 0) +
        'px;width:calc((100vw - ' +
        spacePX +
        'px) / ' +
        column +
        ');'
      "
      :class="[s.isRead ? '' : 'opacity']"
      :dataInfo="s"
      :index="i"
      :key="i"
      v-for="(s, i) in items"
    ></item>
  </view>
</template>

<script>
import item from "./yc-waterfall-item.vue";

export default {
  name: "Falls",
  props: {
    dataList: {
      type: Array,
      default() {
        return [];
      },
    },
    column: {
      type: Number,
      default: 2,
    },
    spaceX: {
      type: Number,
      default: 10,
    },
    spaceY: {
      type: Number,
      default: 10,
    },
    padding: {
      type: Array,
      default() {
        return [12, 12];
      },
    },
  },
  components: {
    item,
  },
  data() {
    return {
      childSizeList: [],
      heightList: [],
      maxHeight: 0,
      isLoad: true,
      items: [],
    };
  },
  computed: {
    spacePX() {
      let { spaceX, column, padding } = this;
      return spaceX * (column - 1) + padding[0] + padding[1];
    },
  },
  mounted() {
    this.init();
    this.items = this.dataList;
  },
  watch: {
    dataList(e) {
      if (!e.length) {
        this.init();
      }
      this.childSizeList = [];

      let _addItems = [];
      e.forEach((opt) => {
        const item = this.items.find((i) => i.id === opt.id);
        if (!item) {
          _addItems.push(opt);
        }
      });
      this.items = [...this.items, ..._addItems];
    },
  },
  methods: {
    init() {
      this.heightList = new Array(this.column).fill(0);
      this.isLoad = true;
      this.maxHeight = 0;
      this.childSizeList = [];
    },
    renderBox(params) {
      // let _this = this;
      // _this.childSizeList.push(params);
      // _this.childSizeList.sort(_this.sortParams).forEach((e) => {
      //   _this.onImgLoad(e);
      // });
      this.onImgLoad(params);
    },
    sortParams(a, b) {
      return a.index - b.index;
    },
    onImgLoad(params) {
      this.isLoad && (this.isLoad = false);
      let _this = this;
      let { height, width, id } = params;
      let { spaceY, spaceX, items, padding, heightList } = _this;
      console.log(heightList);

      const index = items.findIndex((opt) => opt.id === id);

      if (!items[index]?.isRead) {
        let min = Math.min.apply(null, heightList);
        let heightIndex = heightList.indexOf(min);
        let nHeight = spaceY + height;
        let top = heightList[heightIndex] + spaceY;
        let left = (spaceX + width) * heightIndex + padding[0];
        _this.heightList[heightIndex] += nHeight;
        _this.maxHeight = Math.max.apply(null, _this.heightList);

        _this.$set(items[index], "top", top);
        _this.$set(items[index], "left", left);
        _this.$set(items[index], "isRead", true);
      }
    },
    itemClick(item, i) {
      this.$emit("itemClick", item, i);
    },
  },
};
</script>

<style scoped lang="scss">
.l-falls {
  width: 100%;
  position: relative;
}
.item {
  position: absolute;
}
.opacity {
  opacity: 0;
  z-index: -1;
}
</style>
