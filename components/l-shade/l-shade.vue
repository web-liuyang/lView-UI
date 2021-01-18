<template>
  <view><view class="l-shade-box" v-show="show" @click="$emit('click', $event)"></view></view>
</template>

<script>
/**
 * @description 遮罩层
 * @property {Boolean} v-model - 双向绑定-是否显示遮罩层
 * @event {Function} click - 点击事件
 */
export default {
  name: 'l-shade',
  data() {
    return {
      show: false,
    };
  },
  props: {
    value: Boolean,
  },
  watch: {
    value(val) {
      this.show = val;
    },
    show(val) {
      this.$emit('input', val);
    },
  },
};
</script>

<style lang="scss" scoped>
@import '../../style/index.scss';
.l-shade-box {
  position: fixed;
  left: 0;
  bottom: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(#000, 0.1);
}
</style>
