<template>
  <view class="l-text-margin l-icon-box" @click="$emit('click', $event)"><text :class="['l-iconfont l-iconfont-' + mode]" :style="style"></text></view>
</template>

<script>
/**
 * @description icon字体图标
 * @property {String} mode 字体图标的名称
 * @property {String} fontSize 字体图标的大小
 * @property {String} color 字体图标图的颜色
 * @event {Function} click 点击事件
 */
export default {
  name: 'l-icon',
  props: {
    mode: String,
    iconStyle: String,
    fontSize: String,
    color: String,
  },
  methods: {},
  computed: {
    style: ({ fontSize, color }) => {
      const style = {};
      fontSize && (style.fontSize = fontSize);
      fontSize && (style.color = color);
      return style;
    },
  },
};
</script>

<style lang="scss" scoped>
@import '../../style/index.scss';
.l-icon-box {
  display: inline-block;
  margin-right: 5rpx;
}
</style>
