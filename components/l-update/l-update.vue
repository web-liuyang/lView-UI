<template>
  <view v-if="show" class="l-update-box">
    <view class="l-absolute content-box">
      <view class="img-box">
        <image class="img" src="./update.png" />
        <view class="l-title version">V{{ version }}</view>
      </view>
      <view class="l-box container">
        <view class="text" v-for="(item, index) in list" :key="index">{{ index + 1 }}、{{ item }}</view>
      </view>
      <view class="button-box">
        <l-button @click="handleClickUpdate" :background="'linear-gradient(to right, #ff4500 ' + to + '% , #FE6932 0%)'" mode="theme" height="70rpx">立即升级</l-button>
      </view>
    </view>
  </view>
</template>

<script>
import { req } from '../../sdk/l-requrest.js';

/**
 * @description 应用升级框
 * @property {String} version - 版本号 格式1.0.0
 * @property {String} path - 下载地址
 * @property {Boolean} overall - 是整体更新还是打补丁
 * @property {String[]} list - 升级说明列表
 * @property {Boolean} vModel - 是否显示升级框
 */
export default {
  data() {
    return {
      show: this.value, // 是否显示升级框
      to: 0,
      isClickUpdate: false // 是否已经点击过升级，请勿重复点击
    };
  },
  props: {
    version: String,
    list: {
      type: Array,
      default: () => []
    },
    value: {
      type: Boolean,
      default: () => false
    },
    path: String,
  },
  watch: {
    value(val) {
      this.show = val;
      this.$emit('input', val);
    },
  },
  methods: {
    // 立即升级
    async handleClickUpdate() {
      uni.showLoading({
        title: '正在下载...',
        mask: true
      });
      req.ajaxDownload({
        path: this.path,
        beforeAjaxDownload: (data, task) => {
          task.onProgressUpdate(res => {
            this.to = res.progress;
          });
        },
        afterAjaxDownload: data => {
          console.log(data);
          uni.hideLoading();
          plus.runtime.install( data,{ force: true },()=>!this.overall && plus.runtime.restart());
        }
      });
    }
  }
};
</script>

<style lang="scss" scoped>
@import '../../style/index.scss';
.l-update-box {
  position: fixed;
  width: 100%;
  height: 100%;
  z-index: 999;
  &::before{
    content: '';
    position: fixed;
    left: 0;
    top: 0;
    width: 100%;
    height: 100%;
    background: rgba(black, 0.1);
  }
  .content-box {
    width: 80%;
    .img-box {
      position: relative;
      font-size: 0;
      .img {
        width: 100%;
      }
      .version {
        position: absolute;
        top: 100px;
        left: 24px;
        color: #fff;
        &::before {
          content: '发现更新版本';
          position: absolute;
          top: -40px;
          left: 0;
          width: 300px;
          font-size: 44rpx;
        }
      }
    }
    .container {
      background: #fff;
      padding-left: 24px;
      .text {
        line-height: 40px;
      }
    }
    .button-box {
      background: #fff;
      padding: 50rpx 60rpx;
      border-bottom-left-radius: 20rpx;
      border-bottom-right-radius: 20rpx;
    }
  }
}
</style>
