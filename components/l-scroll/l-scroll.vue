<template>
  <scroll-view
    scroll-y
    class="l-scroll-view"
    :style="{ height: scrollHeight + 'px' }"
    :lower-threshold="threshold"
    :status="status"
    :transition="transition"
    :change:lower-threshold="threshold"
    :change:status="status"
    :change:transition="transition"
    @touchstart="touch.touchstart"
    @touchmove="touch.touchmove"
    @touchend="touch.touchend"
    @scrolltolower="touch.scrolltolower"
  >
    <view :class="['vc update-container']" :style="style">
      <view v-show="!status">
        <l-icon mode="down" class="icon-box" :style="{ transform: 'rotateZ(' + (moveY < threshold ? 0 : 180) + 'deg)' }" />
        <text>{{ moveY < threshold ? '下拉刷新' : '松开刷新' }}</text>
      </view>
      <view v-show="status">
        <l-icon mode="star" class="rotate" :style="{ animationPlayState: status ? 'running' : 'paused' }" />
        <text>刷新中</text>
      </view>
    </view>
    <slot />
    <view class="vc load-container" :style="{ marginBottom: threshold / 2 + 'px' }">{{ moreName }}</view>
  </scroll-view>
</template>

<script module="touch" lang="renderjs">
let instance,
oScrollBox,
oTopBox,
y,
moveY = new Proxy({num:0},{
	set(target, property, value, receiver){
		target[property] = value;
		setData('moveY',value)
		return true
	}
});

function setStyle(field,val){
	oTopBox.style[field] = val + 'px';
}

function setData(field,val){
	instance.callMethod('setData',[field,val])
}

function Emit(key,val){
	instance.callMethod('Emit',[key,val])
}

export default{
	watch:{
		status(val) {
			if (!val) {
				Emit('end');
				setStyle('height',0)
			}
		},
	},
	mounted() {
		this.init();
	},
	methods:{
		// 初始化
		init(){
			oScrollBox = document.querySelector('.l-scroll-box');
			oTopBox = oScrollBox.querySelector('.top-box');
			instance = this.$ownerInstance;
			setStyle('height',0)
		},
		// 按下
		touchstart(e) {
			// console.log('touchstart');
			if (this.status) return; // 如果正在刷新就返回，避免重复刷新
			setData('transition',false); // 回弹动画效果
			const { pageY } = e.touches[0];
			y = pageY; // 保存点击的Y轴坐标
			moveY.num = 0; // 重置移动的距离
		},
		// 滑动
		touchmove(e) {
			// console.log('touchmove');
			if (this.status) return; // 如果正在刷新就返回，避免重复刷新
			const { pageY } = e.touches[0];
			moveY.num = pageY - y; // 获取偏移量并保存
			// 上下滑动的距离必须大于0
			if(moveY.num > 0){
				setStyle('height',moveY.num);
			}else{
				setStyle('height',0)
			}
		},
		// 松开
		touchend(e) {
			// console.log('touchend');
			if (this.status) return; // 如果正在刷新就返回，避免重复刷新
			setData('transition',true); // 回弹动画小效果
			 // 等回弹效果设置完成以后在进行操作
			setTimeout(()=>{
				// 滑动的距离大于加载框触发的高度
				if (moveY.num > this.threshold) {
					setStyle('height',this.threshold);
					Emit('update:status', true);
					Emit('start', 'update');
				} else {
					setStyle('height',0)
				}
				moveY.num = 0; // 重置移动的距离
			},15);
		},
		// 上拉加载更多
		scrolltolower() {
			if (this.status) return; // 避免重复刷新
			Emit('update:status', true)
			Emit('update:more', 'loading')
			Emit('start', 'more')
		},
	},
}
</script>
<script>
/**
 * @description 纵向滚动视图
 * @property {String} src - 图片
 * @property {Number | String} scrollHeight - 父组件高度，单位px
 * @property {Number | String} threshold - 距顶部/底部多远时（单位px）,触发下拉/上拉事件
 * @property {Boolean} status - 组件状态 true刷新中，false刷新完成
 * @property {Boolean} more = [loadmore|loading|nomore] 加载前值为loadmore，加载中为loading，没有数据为nomore
 * @event {Function} start - 触发下拉/上拉开始时间
 * @event {Function} end - 触发下拉/上拉结束事件
 */
export default {
  name: 'l-scroll',
  data() {
    return {
      moveY: 0, // 下拉的距离
      transition: false, // 过渡动画状态 -> 下拉后回弹的动画
    };
  },
  props: {
    scrollHeight: Number | String,
    threshold: {
      type: Number | String,
      default: () => 70,
    },
    status: {
      type: Boolean,
      default: () => false,
    },
    more: {
      type: String,
      default: () => 'loadmore',
    },
  },
  methods: {
    // 设置data
    setData([field, value]) {
      this[field] = value;
    },
    // 回掉触发
    Emit([key, value]) {
      this.$emit(key, value);
    },
  },
  computed: {
    moreName: ({ more }) => {
      // 加载前值为loadmore，加载中为loading，没有数据为nomore
      switch (more) {
        case 'loadmore':
          return '上拉加载更多';
        case 'loading':
          return '正在加载中...';
        case 'nomore':
          return '没有更多数据';
      }
    },
    style: ({ transition }) => {
      let style = {};
      style.transition = `height ${transition ? 0.7 : 0}s`;
      return style;
    },
  },
};
</script>
<style lang="scss" scope>
@import '../../style/index.scss';
.l-scroll-view {
  .update-container {
    overflow: hidden;
    .icon-box {
      transition: transform 0.5s linear;
    }
    .rotate {
      animation: rotation 1s linear infinite;
    }
  }
  .load-container {
    height: 50px;
    color: $l-describe-color;
  }
}

@keyframes rotation {
  0% {
    transform: rotateZ(0deg);
  }

  100% {
    transform: rotateZ(360deg);
  }
}
</style>
