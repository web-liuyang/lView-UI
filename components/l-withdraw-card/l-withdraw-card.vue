<template>
	<view class="l-layout l-withdraw-card-box" @click="$emit('click')">
		<view class="list">
			<view class="li">
				<view class="l-title">{{ title }}</view>
				<view class="l-text l-text-margin">{{ text }}</view>
				<view class="l-describe l-text-margin">{{ time }}</view>
				<view class="l-absolute right l-price money">{{ money }}</view>
			</view>
		</view>
	</view>
</template>

<script>
/**
 * @description 提现卡片展示
 * @property {String} title 标题
 * @property {String} text 副标题
 * @property {String} time 时间
 * @property {String} money 金额
 * @event {Function} click 点击事件
 */
export default {
	data() {
		return {};
	},
	props: {
		title: String,
		text: String,
		time: String,
		money: String
	}
};
</script>

<style lang="scss" scoped>
@import 'lview-ui/style/index.scss';
.l-withdraw-card-box {
	.list {
		.li {
			position: relative;
			background: #fff;
			.money {
				right: 24rpx;
			}
		}
	}
}
</style>
