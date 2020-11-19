<template>
	<view>
		<view :class="['l-star-box', hintClass]">
			<l-icon v-if="star" mode="star" color="red" />
			<text :class="['l-text text', textClass]">{{ text }}</text>
		</view>
	</view>
</template>
<script>
/**
 *
 * @description 带星号的提示
 * @property {Boolean} star = [false|true] 是否需要星号默认ture
 * @property {String} text 文本内容
 * @property {String} hintClass 提示框样式
 * @property {String} textClass 文字样式
 */

export default {
	props: {
		star: {
			type: Boolean,
			default: () => true
		},
		text: String,
		hintClass: String,
		textClass: String
	}
};
</script>

<style lang="scss" scoped>
@import '../../style/index.scss';
.l-star-box {
	margin: 0 0 24rpx;
	.text {
		margin: 0 0 0 8rpx;
	}
}
</style>
