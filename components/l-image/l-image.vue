<template>
	<view class="vc l-image-box">
		<image :class="classname" :src="src" :mode="mode" :lazy-load="lazyLoad" @error="error" @load="load" @click="$emit('click')" :style="{ width, height, borderRadius: radius,filter:'blur(' + indistinct +'px)' }" />
		<l-loading v-show="lazyLoad && !over" class="l-absolute l-loading-box" />
	</view>
</template>

<script>
/**
 * @description 图片
 * @property { String } classname 图片的样式名称
 * @property { String } src 图片路径
 * @property { String } mode 图片裁剪、缩放的模式
 * @property { String } width 图片宽
 * @property { String } height 图片高
 * @property { Boolean } lazy-load 图片懒加载，默认启动
 * @property { String } radius 图片圆角
 * @property { String | Number } indistinct 高斯模糊
 * @event {Function} error 图片发生错误触发
 * @event {Function} load 当图片载入完毕时
 * @event {Function} click 点击事件
 */
export default {
	name: 'l-image',
	data() {
		return {
			over: false // 是否加载完成
		};
	},
	props: {
		classname: String,
		src: String,
		mode: String,
		width: String,
		height: String,
    radius: String,
    indistinct:String,
		lazyLoad: {
			type: Boolean,
			default: () => true
		}
	},
	methods: {
		// 当错误发生时
		error(e) {
			this.over = true;
			this.$emit('error', e);
		},
		// 当图片载入完毕时
		load(e) {
			this.over = true;
			this.$emit('load', e);
		}
	}
};
</script>

<style lang="scss" scoped>
@import '../../style/index.scss';
.l-image-box {
	position: relative;
	.l-loading-box {
	}
}
</style>
