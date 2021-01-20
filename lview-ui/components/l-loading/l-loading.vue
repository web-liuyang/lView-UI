<template>
	<view class="l-loading-box"><view class="loading" :style="style" /></view>
</template>

<script>
/**
 * @description loading加载
 * @property {String} size 加载样式的大小,默认50rpx
 */
export default {
	props: {
		size: {
			type: String,
			default: () => '50rpx'
		}
	},
	methods: {},
	computed: {
		style: ({ size }) => {
			const style = {};
			size && (style.width = size) && (style.height = size);
			return style;
		}
	},
	components: {}
};
</script>

<style lang="scss" scoped>
.loading-box {
	.loading {
		background-color: $l-theme-color;
		border-radius: 50%;
		animation: scaleout 1s infinite ease-in-out;
	}
}

@keyframes scaleout {
	0% {
		transform: scale(0);
	}
	100% {
		transform: scale(1);
		opacity: 0;
	}
}
</style>
