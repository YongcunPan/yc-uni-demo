<template>
  <view class="u-flex-1 s-wrap" :class="sId">
    <MescrollBody
      v-if="showScroll"
      ref="mescrollRef"
      :down="downOption"
      :up="upOption"
      :height="scrollViewH"
      @init="mescrollInit"
      @down="downCallback"
      @up="upCallback"
    >
      <slot></slot>
    </MescrollBody>
  </view>
</template>

<script>
import MescrollBody from "@/uni_modules/mescroll-uni/components/mescroll-uni/mescroll-uni.vue";
export default {
  components: {
    MescrollBody,
  },
  props: {
    status: {
      type: String,
      default: "loadmore",
    },
  },
  data() {
    return {
      sId: uni.$u.guid(),
      scrollViewH: 0,
      showScroll: false,
      downOption: {
        auto: false, //是否在初始化后,自动执行downCallback; 默认true
      },
      upOption: {
        textNoMore: "--我也是有底线的--",
        auto: false,
      },
      mescroll: null,
    };
  },
  mounted() {
    this.updateScrollViewH();
  },
  updated() {
    this.$nextTick(() => {
      this.updateScrollViewH();
    });
  },
  watch: {
    status(v) {
      if (v === "loadmore") {
        this.mescroll.endSuccess(10, true);
      }
      if (v === "nomore") {
        this.mescroll.endSuccess(10, false);
      }
    },
  },
  methods: {
    mescrollInit(mescroll) {
      this.mescroll = mescroll;
      this.$emit("initAfter", mescroll);
    },
    downCallback() {
      // 初始化加载
      this.$emit("onDown");
    },
    upCallback() {
      // 加载更多
      this.$emit("onUp");
    },
    updateScrollViewH() {
      const query = uni.createSelectorQuery().in(this);
      query.select(`.${this.sId}`).boundingClientRect();
      query.exec((res) => {
        const scrollViewH = res?.[0]?.height || 0;
        this.scrollViewH = `${scrollViewH}px`;
        this.showScroll = true;
      });
    },
  },
};
</script>
