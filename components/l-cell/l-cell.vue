<template>
	<view class="l-cell-box" @click="$emit('click')">
		<view class="vc l-box box">
			<l-image v-if="src" class="img-box" :src="src" width="40rpx" height="40rpx" />
			<view class="vc l-box container-box">
				<view>{{ title }}</view>
				<view>
					<text v-if="mode === 'select'">{{ text }}</text>
					<l-input v-if="mode === 'input'" :mode="inputMode" v-model="inputValue" :placeholder="placeholder" :starShow="false" style="text-align: right;" borderNone :disabled="disabled"/>
				</view>
			</view>
			<view class="vc arrow-box" @click.stop="$emit('clickIcon')">
				<l-image v-if="icon" :src="icon" width="32rpx" height="32rpx" />
				<l-icon v-else-if="arrow" mode="right" />
			</view>
		</view>
	</view>
</template>

<script>
/**
 * @description 单元格
 * @property {String} src 图片路径
 * @property {String} title 左边内容
 * @property {String} text 右边内容
 * @property {Boolean} arrow 是否显示箭头，默认显示
 * @property {Boolean} icon 自定义图标路径
 * @property {Boolean} mode = [select|input] 单元格类型
 * @property {Boolean} inputMode input框类型
 * @property {String|Number} placeholder 输入框为空时
 * @property {Boolean} disabled 是否禁用输入框
 * @event {Function} click 点击事件
 * @event {Function} clickIcon 点击图标事件
 */
export default {
	name: 'l-cell',
	data() {
		return {
			inputValue: this.value
		};
	},
	watch: {
		inputValue(val) {
			this.$emit('input', val);
		},
		value(val){
			this.inputValue = val
		}
	},
	props: {
		src: String,
		title: String,
		text: String | Number,
		arrow: {
			type: Boolean,
			default: () => true
		},
		icon: String,
		// 输入框
		value: String | Number,
		mode: {
			type: String,
			default: () => 'select'
		},
		inputMode: String,
		placeholder: String | Number,
		disabled: Boolean
	}
};
</script>

<style lang="scss" scoped>
@import '../../style/index.scss';
$height: 110rpx;
.l-cell-box {
	.box {
		justify-content: flex-start;
		height: $height;
		background: #fff;
		.img-box {
			width: 10%;
		}
		.container-box {
			justify-content: space-between;
			width: 95%;
		}
		.arrow-box {
			width: 5%;
		}
	}
}
</style>
