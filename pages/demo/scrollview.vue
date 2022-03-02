<template>
  <MescrollBody
    ref="mescrollRef"
    :top="top"
    :bottom="bottom"
    :down="downOption"
    :up="upOption"
    @init="mescrollInit"
    @down="downCallback"
    @up="upCallback"
  >
    <view class="u-flex-1 u-flex-col u-text-center zzz">
      <u-navbar back-text="返回" title="剑未配妥，出门已是江湖"></u-navbar>
      <view class="u-border-bottom u-p-20" @click="aaa">todo</view>
      <view class="u-flex-1 xxxxx">
        <block class="" v-if="show">
          <block v-for="item in items" :key="item">
            <view class="u-border-bottom u-p-10">{{ item }}</view>
          </block>
        </block>
      </view>
      <view class="u-border-top u-p-20">todo</view>
    </view>
  </MescrollBody>
</template>
<script>
import MescrollBody from "@/uni_modules/mescroll-uni/components/mescroll-body/mescroll-body.vue";
export default {
  components: {
    MescrollBody,
  },
  data() {
    return {
      items: 10,
      mescroll: null,
      downOption: {
        auto: false, //是否在初始化后,自动执行downCallback; 默认true
      },
      upOption: {
        textNoMore: "--我也是有底线的--",
        auto: false,
      },
      show: false,
      top: 0,
      bootom: 0,
    };
  },
  methods: {
    mescrollInit(mescroll) {
      this.mescroll = mescroll;
    },
    downCallback() {
      setTimeout(() => {
        this.items = 10;
        this.mescroll.endSuccess(10, true);
      }, 1000);
    },
    upCallback() {
      setTimeout(() => {
        this.items += 10;
        const hasMore = this.items < 35;
        this.mescroll.endSuccess(10, hasMore);
      }, 1000);
    },
    aaa() {
      const vm = this;
      const query = uni.createSelectorQuery().in(this);
      query.select(`.xxxxx`).boundingClientRect();
      query.exec(([r]) => {
        console.log(r);
        vm.top = `0px`;
        // vm.top = `${r.top}px`;
        uni.getSystemInfo({
          success: (w) => {
            console.log(w.windowHeight, "窗口高度");
            vm.bootom = w.windowHeight - (r.top + r.height) + "px";
            console.log(vm.top, vm.bootom);
            vm.show = true;
          },
        });
      });
    },
  },
  onShow() {
    this.aaa();
  },
};
</script>
<style lang="scss">
mescroll-body {
  flex: 1;
  display: flex;
  flex-direction: column;
}
.xxxxx {
  overflow: hidden;
}
.zzz {
  min-height: 100%;
}
</style>
