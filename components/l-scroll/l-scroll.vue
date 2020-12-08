<template>
  <scroll-view
    scroll-y
    class="l-scroll-box"
    :style="{ height: scrollHeight + 'px' }"
    :lower-threshold="threshold"
    @touchstart="touchstart"
    @touchmove="touchmove"
    @touchend="touchend"
    @scrolltolower="scrolltolower"
  >
    <view :class="['vc top-box', { transition }]" :style="{ height: topMove + 'px' }">
      <slot name="update"><l-image src="https://s.cn.bing.net/th?id=OJ.eEyJ3keSuJmgeg&pid=MsnJVFeeds" width="16px" height="16px" /></slot>
    </view>
    <slot name="content" />
    <view class="vc bottom-box" :style="{ marginBottom: threshold / 2 + 'px' }">
      <slot name="more">{{ moreName }}</slot>
    </view>
  </scroll-view>
</template>

<script>
/**
 * @description 纵向滚动视图
 * @property {String} src - 图片
 * @property {Number | String} scrollHeight - 父组件高度，单位px
 * @property {Number | String} threshold - 距顶部/底部多远时（单位px）,触发下拉/上拉事件
 * @property {Boolean} status - 组件状态 true刷新中，false刷新完成
 * @property {Boolean} more = [loadmore|loading|nomore] 加载前值为loadmore，加载中为loading，没有数据为nomore
 * @event {Function(type)} start - 触发下拉/上拉开始时间
 * @event {Function} end - 触发下拉/上拉结束事件
 */
export default {
  name: 'l-scroll',
  data() {
    return {
      current: 0, // 当前按下Y轴 距离屏幕顶部的像素
      topMove: 0, // 下拉的距离
      transition: false // 过渡动画状态 -> 下拉后回弹的动画
    };
  },
  props: {
    scrollHeight: Number | String,
    threshold: {
      type: Number | String,
      default: () => 70
    },
    status: {
      type: Boolean,
      default: () => false
    },
    more: {
      type: String,
      default: () => 'loadmore'
    }
  },
  watch: {
    status(val) {
      if (!val) {
        this.$emit('end');
        this.topMove = 0;
      }
    }
  },
  methods: {
    // 上拉加载更多
    scrolltolower() {
      if (this.status) return; // 避免重复刷新
      this.$emit('update:status', true);
      this.$emit('update:more', 'loading');
      this.$emit('start', 'more');
    },
    touchstart(e, int) {
      if (this.status) return; // 避免重复刷新
      this.transition = false; // 回弹动画小效果
      const { pageY } = e.touches[0];
      this.current = pageY; // 保存点击的Y轴坐标
    },
    touchmove(e) {
      if (this.status) return; // 避免重复刷新
      const { pageY } = e.touches[0];
      const y = pageY - this.current; // 获取偏移量
      this.topMove = y; // 保存偏移量
    },
    touchend() {
      if (this.status) return; // 避免重复刷新
      this.transition = true; // 回弹动画小效果
      // 是否小于加载框高度
      if (this.topMove < this.threshold) {
        this.topMove = 0;
      } else {
        this.topMove = this.threshold;
        this.$emit('update:status', true);
        this.$emit('start', 'update');
      }
    }
  },
  computed: {
    moreName: ctx => {
      // 加载前值为loadmore，加载中为loading，没有数据为nomore
      switch (ctx.more) {
        case 'loadmore':
          return '上拉加载更多';
        case 'loading':
          return '正在加载中...';
        case 'nomore':
          return '没有更多数据';
      }
    }
  }
};
</script>
<style lang="scss" scope>
@import '../../style/index.scss';
.l-scroll-box {
  .transition {
    transition: height 0.7s;
  }
  .top-box {
    overflow: hidden;
  }
  .bottom-box {
    height: 50px;
    color: $l-describe-color;
  }
}
</style>
