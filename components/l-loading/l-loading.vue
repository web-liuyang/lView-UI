<template>
	<view class="spinner-box"><view class="spinner" :style="{ width: size, height: size }" /></view>
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
	computed: {},
	components: {}
};
</script>

<style lang="scss" scoped>
.spinner-box {
	.spinner {
		background-color: $l-theme-color;
		border-radius: 50%;
		animation: scaleout 1s infinite ease-in-out;
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
}
</style>
