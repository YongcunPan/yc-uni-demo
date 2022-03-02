<template>
  <view class="u-flex-1 u-flex-col">
    <view class="u-border u-p-30" @click="toPickerDate">uView Calendar</view>
    <view class="u-border u-p-30" @click="toScrollPage">toScrollPage</view>
    <!-- <view>
      <uni-calendar
        :insert="true"
        :start-date="'2000-3-2'"
        :end-date="'2032-5-20'"
        @change="changeData"
      />
    </view> -->
    <view class="u-border u-flex-1 u-p-10">
      <YcCalendar @change="onChange" />
    </view>
    <u-loadmore @loadmore="getData" />
    <u-calendar v-model="show" :mode="mode" @change="onChange" />
  </view>
</template>

<script>
import YcCalendar from "@/components/yc-calendar";
export default {
  components: {
    YcCalendar,
  },
  data() {
    return {
      show: false,
      mode: "date",
      items: [],
    };
  },
  methods: {
    toPickerDate() {
      this.show = true;
    },
    onChange(v) {
      console.log(v);
    },
    getData() {
      for (let i = 0; i < 12; i++) {
        const w = this.$u.random(200, 400);
        const h = this.$u.random(300, 600);

        let item = {
          id: this.$u.guid(),
          icon: `http://placehold.it/${w}x${h}`,
          title: this.$u.random(300, 600),
        };
        this.items.push(item);
      }
    },
    clear() {
      // this.$refs.uWaterfall.clear();
    },
    toScrollPage() {
      this.$u.route("/pages/demo/home");
    },
    changeData(e) {
      console.log(e);
    },
  },
  onShow() {
    // this.clear();
    this.getData();
  },
};
</script>

<style lang="scss" scoped>
.water-item {
  padding: 10rpx;
}
</style>
