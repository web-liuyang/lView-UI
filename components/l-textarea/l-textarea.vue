<template>
	<view class="l-border l-textarea-box">
		<textarea
			:value="inputValue"
			:placeholder="placeholder"
			:placeholder-style="placeholderStyle"
			:placeholder-class="placeholderClass"
			:disabled="disabled"
			:maxlength="maxlength"
			@input="input"
		/>
	</view>
</template>

<script>
/**
 * @description 多行输入框
 * @property {String} placeholder 输入框为空时占位符
 * @property {String} placeholderStyle 	指定 placeholder 的样式
 * @property {String} placeholderClass 指定 placeholder 的样式类
 * @property {Boolean} disabled 是否禁用
 * @property {String} maxlength 	最大输入长度，设置为 -1 的时候不限制最大长度
 * @event {Function} input 当键盘输入时，触发 input 事件
 */
export default {
	data() {
		return {
			inputValue: ''
		};
	},
	props: {
		value: String,
		placeholder: String,
		placeholderStyle: String,
		placeholderClass: String,
		disabled: Boolean,
		maxlength: String | Number
	},
	watch: {
		inputValue(val) {
			this.$emit('input', val);
		},
		value(val) {
			this.inputValue = val;
		}
	},
	methods: {
		input(e) {
			const { value } = e.detail;
			this.inputValue = value
		}
	},
	computed: {},
	components: {}
};
</script>

<style lang="scss" scoped>
@import '../../style/index.scss';
.l-textarea-box {
	width: 100%;
}
</style>
