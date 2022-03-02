<template>
  <view class="l-module" @click="itemClick">
    <image
      class="l-module-img"
      v-if="isLoad"
      @load="getImgData"
      @error="getImgDataError"
      :src="dataInfo.icon"
      mode="widthFix"
    ></image>
    <view class="">{{ index + 1 }}</view>
  </view>
</template>

<script>
export default {
  props: {
    dataInfo: {
      type: Object,
      default() {
        return {};
      },
    },
    index: Number,
  },
  data() {
    return {
      isLoad: true,
      number: 0,
    };
  },
  methods: {
    getImgDataError(e) {
      this.getImgData();
    },
    getImgData(e) {
      let _this = this;
      try {
        let index = _this.index;
        let createSelectorQuery = uni.createSelectorQuery().in(_this);
        createSelectorQuery
          .select(".l-module")
          .fields(
            {
              size: true,
            },
            (res) => {
              let { height, width } = res;
              if (width && height) {
                _this.$emit("renderBox", {
                  index,
                  height,
                  width,
                  id: _this.dataInfo.id,
                });
              } else {
                _this.anewLoadImg();
              }
            }
          )
          .exec();
      } catch (e) {
        _this.anewLoadImg();
        throw new Error(e);
      }
    },
    // 渲染超出3s就停止继续渲染
    anewLoadImg() {
      ++this.number;
      if (this.number > 30) {
        return;
      }
      this.isLoad = false;
      setTimeout(() => {
        this.isLoad = true;
      }, 100);
    },
    itemClick() {
      this.$emit("itemClick", this.dataInfo, this.index);
    },
  },
};
</script>

<style scoped lang="scss">
.l-module {
  z-index: 6;
  width: 100%;
  border-radius: 12rpx;
  color: #3c3c43;
  overflow: hidden;
  font-size: 22rpx;
  background-color: #f8fbff;
  margin-bottom: 10px;
  &-img {
    width: 100%;
    background-color: #f1f1f1;
    display: block;
    border-radius: 12rpx;
  }

  &-title {
    padding: 14rpx 10rpx 6rpx;
    font-size: 24rpx;
    line-height: 34rpx;
    word-break: break-all;
    display: -webkit-box;
    -webkit-box-orient: vertical;
    -webkit-line-clamp: 2;
    overflow: hidden;
    max-height: 64rpx;
  }
}
</style>
