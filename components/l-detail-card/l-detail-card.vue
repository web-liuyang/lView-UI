<template>
  <view class="l-layout l-detail-card-box">
    <view class="l-title">{{ title }}</view>
    <view class="detail-list">
      <view class="vc l-text l-text-margin detail-li" v-for="(item, index) in list" :key="index">
        <text class="name">{{ item.name }}</text>
        <text class="l-right value">{{ item.value }}</text>
      </view>
      <slot />
    </view>
  </view>
</template>

<script>
export default {
  name: 'l-detail-card',
  data() {
    return {};
  },
  props: {
    title: String | Number,
    list: {
      type: Array,
      default: () => [],
    },
  },
};
</script>

<style lang="scss" scoped>
.l-detail-card-box {
  background: #fff;
  .detail-list {
    .detail-li {
      align-items: flex-start;
      .name {
        flex: 1;
      }
      .value {
        flex: 2;
      }
    }
  }
}
</style>
