<template>
  <view class="">
    <view class="u-flex">
      <view class="u-flex-1" @click="current = 0">xxxxxxxxxxx</view>
      <view class="u-flex-1" @click="current = 1">-----------</view>
    </view>
    <block>
      <view class="" v-if="!current">
        <yc-waterfall :dataList="items" @itemClick="itemClick" />
        <view class="u-p-10 u-text-center" @click="getData">加载更多</view>
      </view>
      <view class="" v-else>todo</view>
    </block>
  </view>
</template>

<script>
import ycWaterfall from "../../components/yc-waterfall.vue";

export default {
  components: { ycWaterfall },
  data() {
    return {
      current: 0,
      items: [],
    };
  },
  methods: {
    getData() {
      let res = [];
      for (let i = 0; i < 12; i++) {
        const w = this.$u.random(200, 400);
        const h = this.$u.random(300, 600);

        let item = {
          id: this.$u.guid(),
          icon: `http://placehold.it/${w}x${h}`,
          title: this.$u.random(300, 600),
        };
        res.push(item);
      }
      this.items = [...this.items, ...res];
    },
    itemClick(item, index) {
      console.log(item, index);
    },
  },
  onLoad() {
    this.getData();
  },
};
</script>

<style lang="scss" scoped></style>
