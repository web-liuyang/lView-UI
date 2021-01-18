<template>
  <view class="l-nav-box">
    <view class="status-bar-box" :style="{ background }">
      <view class="status-bar" :style="{ height: statusBarHeight + 'px', background }"></view>
      <view :class="['vc nav-bar', classname]" :style="{ background, color }">
        <view class="vc left" v-if="isBack" @click="clickLeft">
          <l-icon mode="left" />
          <text>{{ left }}</text>
        </view>
        <view class="vc l-title center" @click="clickCenter">{{ center || $Route.style.navigationBarTitleText || '' }}</view>
        <view class="vc right" @click="clickRight">{{ right }}</view>
      </view>
    </view>
    <view :style="{ width: '100%', height: statusBarHeight + 44 + 'px' }"></view>
  </view>
</template>

<script>
/**
 * @description 自定义导航栏
 * @property {String} background 导航背景色
 * @property {String} color 导航字体颜色
 * @property {String} classname 导航栏样式
 * @property {Boolean} is-back 是否显示左边内容,默认false
 * @property {String} left 左边内容
 * @property {String} center 中间内容，默认是navigationBarTitleText字段标题
 * @property {String} right 右边内容
 * @event {Function} clickLeft 点击左边内容
 * @event {Function} clickCenter 点击内容
 * @event {Function} clickRight 点击右边内容
 */
export default {
  name: 'l-navbar',
  data() {
    return {
      statusBarHeight: Object.freeze(uni.getSystemInfoSync().statusBarHeight), // 状态栏高度
    };
  },
  props: {
    background: String,
    color: String,
    classname: String,
    isBack: Boolean,
    left: String,
    center: String,
    right: String,
  },
  methods: {
    /* 点击左边内容 */
    clickLeft() {
      this.$Router.back();
      this.$emit('clickLeft');
    },
    /* 点击中间内容 */
    clickCenter() {
      this.$emit('clickCenter');
    },
    /* 点击右边内容 */
    clickRight() {
      this.$emit('clickRight');
    },
  },
  computed: {},
  components: {},
};
</script>

<style lang="scss" scoped>
@import '../../style/index.scss';
.l-nav-box {
  width: 100%;
  .status-bar-box {
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    z-index: 99;
    background: #fff;
    .status-bar {
      width: 100%;
    }
    .nav-bar {
      position: relative;
      width: 100%;
      height: $l-nav-height;

      .left {
        position: absolute;
        top: 50%;
        left: 20rpx;
        transform: translateY(-50%);
        // .img {
        // 	margin: 0 10rpx 0 0;
        // }
      }
      .center {
      }
      .right {
        position: absolute;
        top: 50%;
        right: 20rpx;
        transform: translateY(-50%);
      }
    }
  }
}
</style>
