<template>
  <view class="u-flex-1 u-flex-col">
    <u-navbar back-text="返回" title="剑未配妥，出门已是江湖"></u-navbar>
    <view class="u-text-center" @click="changeStatus">todo</view>
    <YcScrollview
      :status="pageStatus"
      @initAfter="initAfter"
      @onUp="onUp"
      @onDown="onDown"
    >
      <uni-list :border="false">
        <block v-for="item in items" :key="item">
          <uni-list-item :border="false" direction="column">
            <block slot="header"></block>
            <view slot="body" class="u-text-center u-p-80 u-border">
              {{ item }}
            </view>
          </uni-list-item>
        </block>
      </uni-list>
    </YcScrollview>
    <view class="u-text-center u-p-20">xxxx</view>
  </view>
</template>
<script>
// import MescrollMixin from "@/uni_modules/mescroll-uni/components/mescroll-uni/mescroll-mixins.js";
import YcScrollview from "@/components/yc-scrollview.vue";
export default {
  // mixins: [MescrollMixin], // 使用mixin
  components: {
    YcScrollview,
  },
  data() {
    return {
      items: 20,
      mescroll: null,
      pageStatus: "loadmore",
    };
  },
  mounted() {},
  updated() {},
  methods: {
    changeStatus() {
      this.pageStatus = this.$u.guid();
    },
    initAfter(mescroll) {
      this.mescroll = mescroll;
    },
    onUp() {
      // 加载更多
      console.log("xxxx加载更多");
      this.pageStatus = "loading";
      setTimeout(() => {
        this.items += 20;
        const hasMore = this.items < 400;
        this.pageStatus = hasMore ? "loadmore" : "nomore";
      }, 1000);
    },
    onDown() {
      // 初始化加载
      console.log("xxx初始化加载");
      this.pageStatus = "loading";
      setTimeout(() => {
        this.items = 20;
        this.pageStatus = "loadmore";
      }, 1000);
    },
  },
};
</script>
<style lang="scss">
.s-wrap {
  overflow: hidden;
}
.u-border {
  background: #ddd;
}
.uni-list-item__container {
  padding: 0 !important;
}
</style>
