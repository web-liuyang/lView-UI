<template>
	<view class="l-text-margin l-icon-box" @click="$emit('click')"><text :class="['l-iconfont l-iconfont-' + mode]" :style="{ fontSize, color }"></text></view>
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
		color: String
	},
	methods: {}
};
</script>

<style lang="scss" scoped>
@import '../../style/index.scss';
.l-icon-box {
	display: inline-block;
	margin-right: 5rpx;
}
</style>
