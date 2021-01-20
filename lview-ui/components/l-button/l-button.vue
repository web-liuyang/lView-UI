<template>
  <view>
    <button
      :class="['l-button-' + mode, classname]"
      type="default"
      :form-type="formType"
      :disabled="disabled"
      :plain="plain"
      :hover-class="hoverClass"
      @click="$emit('click',$event)"
      :style="style"
    >
      <slot />
    </button>
  </view>
</template>

<script>
/**
 * @description 按钮
 * @property {String} classname 按钮样式
 * @property {String} mode = [common|theme|theme-linear|success|error|warning|primary] 按钮样式形态
 * @property {String} formType = [submit|reset] 用于 <form> 组件，点击分别会触发 <form> 组件的 submit/reset 事件
 * @property {Boolean} disabled 是否禁用
 * @property {Boolean} plain 背景是否镂空
 * @property {Boolean} hoverClass = [none]  取消点击态
 * @property {String} width 按钮宽
 * @property {String} height 按钮高
 * @property {String} radius 按钮圆角
 * @property {String} background 按钮背景
 * @property {String} font-size 按钮字体大小
 * @event {Function} click 点击事件
 */
export default {
  name: 'l-button',
  data() {
    return {};
  },
  props: {
    classname: String,
    mode: {
      type: String,
      default: () => 'common',
    },
    formType: String,
    disabled: Boolean,
    plain: Boolean,
    hoverClass: String,
    width: String,
    height: String,
    radius: String,
    background: String,
    fontSize: String,
  },
  methods: {},
  computed: {
    style: ({ width, height, background, radius, fontSize }) => {
      const style = {};
      width && (style.width = width);
      height && (style.height = height);
      background && (style.background = background);
      radius && (style.borderRadius = radius);
      fontSize && (style.fontSize = fontSize);
      return style;
    },
  },
};
</script>

<style lang="scss" scoped>
@import '../../style/index.scss';
</style>
