<template>
  <view class="l-popup-box">
    <view class="popup-box">
      <view class="shade" v-show="show" @click="handleClickShade(false)"></view>
      <view class="slot" :style="style">
        <view class="container"><slot /></view>
      </view>
    </view>
  </view>
</template>

<script>
import { node } from '../../sdk/l-common.js';
/**
 * popup弹出层
 * @description popup弹出层
 * @property { String } mode = [top|center|bottom|region] - 弹出方式
 * @property { String } height - 高度
 * @property { String } radius - 圆角
 */
export default {
  name: 'l-popup',
  data() {
    return {
      show: this.value,
      height: 0,
    };
  },
  props: {
    value: Boolean,
    mode: {
      type: String,
      default: () => 'bottom',
    },
    radius: String,
  },
  watch: {
    value(val) {
      this.show = val;
    },
    show(val) {
      this.$emit('input', val);
    },
  },
  async mounted() {
    const data = await node('.container', this);
    this.height = data.height + 'px';
  },
  methods: {
    // 点击遮罩层
    handleClickShade(status) {
      this.show = status;
      // this.$emit('input',status)
    },
  },
  computed: {
    style: (ctx) => {
      const style = {};
      style.height = ctx.show ? ctx.height : 0;
      style.borderRadius = ctx.radius ? `${ctx.radius} ${ctx.radius} 0 0` : 0;
      return style;
    },
  },
};
</script>

<style lang="scss" scoped>
@import '../../style/index.scss';

.l-popup-box {
  position: fixed;
  left: 0;
  bottom: 0;
  width: 100%;
  overflow: hidden;
  .popup-box {
    .shade {
      position: fixed;
      left: 0;
      bottom: 0;
      width: 100%;
      height: 100%;
      background-color: rgba(#000, 0.1);
      z-index: -1;
    }
    .slot {
      background-color: #fff;
      z-index: 99;
      transition: height 0.3s ease-out;
    }
  }
}
</style>
