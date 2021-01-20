<template>
  <view class="l-box">
    <!-- 省市区 -->
    <picker-view v-if="mode === 'region'" class="picker-view-region" :value="PickerIndex" @change="handleChangePickerCity">
      <picker-view-column>
        <view class="vc picker-view-column" v-for="(item, index) in provinceList" :key="index">{{ item.name }}</view>
      </picker-view-column>
      <picker-view-column>
        <view class="vc picker-view-column" v-for="(item, index) in cityList" :key="index">{{ item.name }}</view>
      </picker-view-column>
      <picker-view-column>
        <view class="vc picker-view-column" v-for="(item, index) in regionList" :key="index">{{ item.name }}</view>
      </picker-view-column>
    </picker-view>

    <!-- 单列 -->
    <picker-view v-if="mode === 'selector'" class="picker-view-selector" :value="PickerIndex" @change="handleChangePickerSelector">
      <picker-view-column>
        <view class="vc picker-view-column" v-for="(item, index) in selectorList" :key="index">{{ item.name }}</view>
      </picker-view-column>
    </picker-view>

    <view class="l-common-padding"><l-button mode="theme" @click="handleClickSure">确定</l-button></view>
  </view>
</template>

<script>
import cityCode from './city.json';
const cityparams = (value) => {
  const provinceList = cityCode[value[0]];
  const cityList = cityCode[value[0]].child[value[1]];
  const regionList = cityCode[value[0]].child[value[1]].child[value[2]];
  const { child: provinceChild, ...province } = provinceList;
  const { child: cityChild, ...city } = cityList;
  const { child: regionChild, ...region } = regionList;
  return [province, city, region];
};
/**
 *
 * @description 自定义picker弹出层选择器
 * @property { String } mode = [selector|multiSelector|region] 选择器
 * @property { Number[] } pickerIndex 初始值下标
 * @property { Object[] } selectorList 单项选择器数组
 *
 * @event { Function } handleClickSure 点击确认后的回调
 *
 */
export default {
  name: 'l-picker',
  data() {
    return {
      show: false,
      cityCode: Object.freeze(cityCode), // 城市数据
      PickerIndex: this.pickerIndex,
    };
  },
  props: {
    value: Boolean,
    mode: String,
    pickerIndex: {
      type: Array,
      default: () => [0, 0, 0],
    },
    selectorList: Array,
  },
  watch: {
    value(val) {
      this.show = val;
    },
    show(val) {
      this.$emit('input', val);
    },
    pickerIndex(val) {
      this.PickerIndex = val;
    },
    PickerIndex(val) {
      console.log(val);
      this.$emit('update:pickerIndex', val);
    },
  },
  methods: {
    // 当滚动选择，value 改变时触发 change 事件
    handleChangePickerCity(e) {
      const { value } = e.detail;
      const pickerIndex = this.PickerIndex;
      if (pickerIndex[0] !== value[0]) {
        value[1] = 0;
        value[2] = 0;
      } else if (pickerIndex[0] === value[0] && pickerIndex[1] !== value[1]) {
        value[2] = 0;
      }
      this.PickerIndex = value;
      // this.$emit('update:pickerIndex', value);
    },
    // 单项选择器
    handleChangePickerSelector(e) {
      const { value } = e.detail;
      this.PickerIndex = value;
      // this.$emit('update:pickerIndex', value);
    },
    // 确定
    handleClickSure() {
      this.show = false;
      const pickerIndex = this.PickerIndex;
      switch (this.mode) {
        case 'region':
          this.$emit('handleClickSure', cityparams(pickerIndex), pickerIndex);
          break;
        case 'selector':
          this.$emit('handleClickSure', this.selectorList[pickerIndex[0]], pickerIndex);
          break;
      }
    },
  },
  computed: {
    provinceList: (ctx) => cityCode,
    cityList: (ctx) => cityCode[ctx.PickerIndex[0]].child,
    regionList: (ctx) => cityCode[ctx.PickerIndex[0]].child[ctx.PickerIndex[1]].child,
  },
  components: {},
};
</script>

<style lang="scss" scoped>
picker-view {
  margin: 20rpx 0;
  .picker-view-column {
    font-size: 30rpx;
  }
}
.picker-view-region,
.picker-view-selector {
  height: 365rpx;
}
</style>
