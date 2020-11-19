<template>
	<view class="tabbar">
		<view class="vc tabbar-list">
			<view :class="['list-li', { sel: index === idx }]" v-for="(item, idx) in tabbar" :key="idx" @click="handleClickPath(item, idx)" :style="'width:' + liWidth + '%;'">
				<block v-if="item.midIconPath"><l-image class="img-box" :src="item.midIconPath" width="90rpx" height="90rpx"/></block>
				<block v-else>
					<view :class="['vc li-img', { hint: hintList.includes(idx) }]"><image :src="index === idx ? item.selectedIconPath : item.iconPath" /></view>
					<view class="vc li-text l-common">{{ item.text }}</view>
				</block>
			</view>
		</view>
	</view>
</template>

<script>
/**
 * @description 自定义tabbar
 * @property {Array} tabbar tabbar列表格式{ selectedIconPath , iconPath , text}
 * @property {Number} index 当前下标，使用.sync修饰符进行双向绑定
 * @property {Array} hintList 提示红点列表 item填tabbar下标
 * @event {Function(item,index)} handleClickItem 点击某个底部，两个参数：当前点击数据与下标
 */
export default {
	props: {
		tabbar: Array,
		index: Number,
		hintList: {
			type: Array,
			default: () => []
		}
	},
	methods: {
		// 点击底部
		handleClickPath(item, index) {
			uni.setNavigationBarTitle({ title: item.text }); //动态设置标题
			this.$emit('handleClickItem', item, index);
			this.$emit('update:index', index);
		}
	},
	computed: {
		// 计算每个tabbar-li的宽度
		liWidth: ctx => 100 / ctx.tabbar.length
	},
	components: {}
};
</script>

<style lang="scss" scoped>
	@import '../../style/index.scss';
.tabbar {
	height: $l-tabbar-height;
	.tabbar-list {
		position: fixed;
		bottom: 0;
		width: 100%;
		padding: 12rpx 0;
		background: #fff;
		overflow: hidden;
		border-top: 2rpx #eee solid;
		z-index: 99;
		// 未选中的样式
		.list-li {
			// 宽度已经计算
			text-align: center;
			.img-box {
				position: fixed;
				left: 50%;
				bottom: 10rpx;
				transform: translateX(-50%);
				padding: 10rpx;
				background: #fff;
				border-radius: 50%;
			}
			.li-img {
				position: relative;
				width: 100%;
				image {
					width: 38rpx;
					height: 38rpx;
				}
				&.hint::after {
					position: absolute;
					top: -5rpx;
					right: 50rpx;
					transform: translateX(calc(-50% - 9rpx));
					display: block;
					content: '';
					width: 14rpx;
					height: 14rpx;
					background: $l-theme-color;
					border-radius: 50%;
				}
			}

			.li-text {
				width: 100%;
				color: $l-tabbar-color;
			}
			// 已选中的样式
			&.sel {
				.li-text {
					color: $l-theme-color;
				}
			}
		}
	}
}
</style>
