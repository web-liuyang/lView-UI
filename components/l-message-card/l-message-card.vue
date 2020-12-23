<template>
	<view class="l-message-card-box">
		<view class="l-box l-layout" @click="$emit('click')">
			<view class="vc title-box">
				<view class="l-title">{{ title }}</view>
				<view class="l-text">{{ type }}</view>
			</view>
			<view class="l-text-margin l-describe date-box">时间：{{ time }}</view>
			<view class="l-box-margin l-ellipsis l-text desc-box">{{ content }}</view>
			<view class="vc see-detail-box">
				<text>查看详情</text>
				<l-icon mode="right" color="#666" />
			</view>
		</view>
	</view>
</template>

<script>
/**
 * @description 消息卡片展示
 * @property {String | Number} title 标题
 * @property {String | Number} type 消息状态
 * @property {String | Number} time 时间
 * @property {String | Number} content 内容
 * @event {Function} click 点击事件
 */
export default {
	name: 'l-message-card',
	props: {
		title: String | Number,
		type: String | Number,
		time: String | Number,
		content: String | Number
	},
	methods: {},
	computed: {},
	components: {}
};
</script>

<style lang="scss" scoped>
@import '../../style/index.scss';
.l-message-card-box {
	width: 100%;
	.title-box {
		justify-content: space-between;
	}
	.date-box {
		color: $l-color;
	}
	.desc-box {
		color: $l-describe-color;
		-webkit-line-clamp: 2;
	}
	.see-detail-box {
		justify-content: space-between;
		border-top: $l-control-border;
		color: $l-describe-color;
	}
}
</style>
