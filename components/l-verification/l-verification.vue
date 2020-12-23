<template>
	<view class="verification">
		<l-button :classname="classname" :mode="mode" @click="$emit('click')">{{ (num && num + '秒后重新获取') || '获取验证码' }}</l-button>
	</view>
</template>

<script>
import cache from '../../sdk/ly-cache.js';

/**
 *
 * @description 倒计时
 * @property {String} classname class样式表
 * @property {Number} time 倒计时时间默认60秒
 * @property {String} mode = [theme|success|error|warning|primary] 按钮样式形态
 * @property {Boolean} plain 背景是否镂空
 * @property {Boolean} hoverClass = [none]  取消点击态
 * @event {Function} click click点击事件
 * @event {Function} start 开始倒计时触发
 * @event {Function} end 倒计时结束触发
 */
export default {
	name: 'l-verification',
	data() {
		return {
			num: '',
			disabled: false
		};
	},
	props: {
		classname: String,
		time: {
			type: Number,
			default: () => 60
		},
		mode: String,
		formType: String,
		plain: Boolean,
		hoverClass: String
	},
	created() {
		this.getCountdownTime();
	},
	methods: {
		// 获取倒计时时间
		getCountdownTime() {
			const { data } = cache.get('countdownTime');
			if (!data) return;
			const time = Math.floor(new Date().getTime() / 1000);
			this.num = this.time - (time - data.num);
			this.num--;
			const timer = setInterval(() => {
				if (this.num <= 0) {
					this.$emit('end');
					clearInterval(timer);
					this.num = '';
				} else {
					this.num--;
				}
			}, 1000);
		},
		// 获取验证码
		handleClickGetCode() {
			const { data } = cache.get('countdownTime');
			if (data) return data;
			this.$emit('start');
			cache.set('countdownTime', { num: Math.floor(new Date().getTime() / 1000) }, this.time);
			this.getCountdownTime();
		}
	}
};
</script>

<style lang="scss" scoped>
	@import '../../style/index.scss';
</style>
