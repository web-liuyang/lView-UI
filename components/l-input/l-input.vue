<template>
	<view>
		<l-star v-if="starShow" :star="star" :text="text" />
		<view style="position: relative;" @click="$emit('clickInput')">
			<l-image class="icon-box" :src="icon" :lazyLoad="false" width="32rpx" height="32rpx" />
			<input
				v-if="mode === 'money'"
				:class="[{ 'l-input-icon': icon }, { 'l-border-none': borderNone }, classname]"
				:disabled="disabled"
				:type="type"
				:value="inputValue"
				:name="name"
				:confirm-type="confirmType"
				:placeholder="placeholder"
				:placeholder-class="placeholderClass"
				:placeholder-style="placeholderStyle"
				:maxlength="maxlength"
				@input="handleInput"
				@confirm="confirm"
			/>
			<block v-else-if="mode === 'code'">
				<input
					:class="['l-input-code', classname]"
					:disabled="disabled"
					:type="type"
					:value="inputValue"
					:name="name"
					:confirm-type="confirmType"
					:placeholder="placeholder"
					:placeholder-class="placeholderClass"
					:placeholder-style="placeholderStyle"
					:maxlength="maxlength"
					@input="handleInput"
					@confirm="confirm"
				/>
				<l-image class="l-absolute code" width="32rpx" height="32rpx" :lazyLoad="false" src="/static/icon/code.png" @click="clickCode" />
			</block>
			<input
				v-else
				:class="[{ 'l-input-icon': icon }, { 'l-border-none': borderNone }, classname]"
				:disabled="disabled"
				:type="type"
				:value="inputValue"
				:name="name"
				:confirm-type="confirmType"
				:placeholder="placeholder"
				:placeholder-class="placeholderClass"
				:placeholder-style="placeholderStyle"
				:maxlength="maxlength"
				@input="handleInput"
				@confirm="confirm"
			/>
		</view>
	</view>
</template>

<script>
import { reglist } from '@/utils/ly-regexp/ly-regexp.js';

/**
 *
 * @description input框
 * @property {Boolean} starShow = [false|true] 是否显示提示默认true
 * @property {Boolean} star = [false|true] 是否需要星号默认true
 * @property {String} text 文本内容
 * @property {String} mode =[money,code] 是否启动过滤
 * @property {String} type 输入框的类型，默认text
 * @property {Boolean} disabled 是否禁用
 * @property {String} v-model 用于双向绑定输入框的值
 * @property {String} value 输入框默认值
 * @property {String} placeholder 值为空的时候显示的提示
 * @property {String} name 输入框的名称
 * @property {String} confirm-type 设置键盘右下角按钮的文字，仅在 type="text" 时生效。
 * @property {Number | String} maxlength 最大输入长度，设置为 -1 的时候不限制最大长度
 * @property {String} borderNone 是否无边框
 * @property {ImageURIString} icon icon图标路径
 * @property {ClassString} classname input框class样式名称
 * @property {ClassString} placeholder-class placeholder-class样式名称
 * @property {String} placeholder-style placeholder-style样式
 * @event {Function} input 输入框框事件
 * @event {Function} clickInput 点击input框事件
 * @event {Function} confirm 点击完成按钮时候触发
 * @event {Function} clickCode 点击扫码按钮
 *
 */
export default {
	name: 'l-input',
	data() {
		return {
			inputValue: this.value
		};
	},
	props: {
		starShow: {
			type: Boolean,
			default: () => true
		},
		star: String,
		text: String,
		value: String | Number,
		mode: String,
		type: String,
		confirmType: String,
		disabled: Boolean,
		classname: String,
		placeholderClass: String,
		placeholderStyle: String,
		placeholder: String,
		name: String,
		hint: Object,
		icon: String,
		maxlength: String | Number,
		borderNone: Boolean
	},
	watch: {
		value(val) {
			this.inputValue = val;
		}
	},
	methods: {
		// input内容发生变化
		handleInput(e) {
			const { value } = e.detail;
			this.inputValue = value;
			if (this.mode === 'money') {
				const isLegal = reglist.price.test(value);
				!isLegal && this.$nextTick(() => (this.inputValue = ''));
			}
			this.$emit('input', value);
		},
		// 扫描二维码
		clickCode() {
			// 调起条码扫描
			uni.scanCode({
				// onlyFromCamera: true,
				scanType: ['barCode'],
				success: res => {
					this.$emit('clickCode', res);
				}
			});
		},
		// 确定
		confirm(e) {
			this.$emit('confirm', e);
		}
	},
	components: {}
};
</script>

<style lang="scss" scoped>
@import '../../style/index.scss';
.icon-box {
	position: absolute;
	top: 50%;
	left: 40rpx;
	transform: translateY(-50%);
}
.l-input-icon {
	padding: 0 25rpx 0 100rpx;
}

.l-input-code {
	padding: 0 80rpx 0 25rpx;
}
.code {
	left: auto;
	right: 40rpx;
}
</style>
