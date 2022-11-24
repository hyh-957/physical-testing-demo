<template>
  <view class="form-container">
    <view class="form-control line-1">
      <view class="grade-select">
        <view class="label">年级: </view>
        <picker class="picker" @change="bindPickerChange" :value="index" :range="array">
          <view class="picker-inside">
            <view class="picker-inside-value">{{ array[index] }}</view>
            <view class="arrow"></view>
          </view>

        </picker>
      </view>
      <view class="gender-radio">
        <view class="label">性别: </view>
        <radio-group class="radio-group" @change="radioChange">
          <label class="radio-label">
            <radio value="1" :checked="current === '1'" />男
          </label>
          <label class="radio-label">
            <radio value="0" :checked="current === '0'" />女
          </label>
        </radio-group>
      </view>
    </view>

    <form @reset="reset">
      <view class="form-control line-2">
        <text class="column-header">应测项目</text>
        <button class="reset-button" form-type="reset">重置</button>
      </view>

      <view class="form-control">
        <text>身高(厘米)</text>
        <input type="number" v-model="height" placeholder="请输入" />
      </view>

      <view class="form-control">
        <text>体重(千克)</text>
        <input type="number" v-model="weight" placeholder="请输入" />
      </view>

      <view class="form-control">
        <text>肺活量(毫升)</text>
        <input type="number" v-model="vitalCapacity" placeholder="请输入" />
      </view>

      <view class="form-control">
        <text>立定跳远(厘米)</text>
        <input type="number" v-model="jump" placeholder="请输入" />
      </view>

      <view class="form-control">
        <text>座位体前屈(厘米)</text>
        <input type="number" v-model="sitAndReach" placeholder="请输入" />
      </view>

      <view class="form-control">
        <text>50米跑(秒)</text>
        <input type="number" v-model="the50MeterRun" placeholder="请输入" />
      </view>

      <view class="form-control">
        <text>1000米跑(秒)</text>
        <input type="number" v-model="the1000MeterRun" placeholder="请输入" />
      </view>

      <view class="form-control">
        <text>引体向上(次)</text>
        <input type="number" v-model="pullUp" @blur="count" placeholder="请输入" />
      </view>

    </form>



  </view>


</template>

<script lang="ts">

export default {
  emits: ['outputCount'],
  data() {
    return {
      array: ['大一', '大二', '大三', '大四'],
      index: 0,
      current: "1",
      height: 180,
      weight: null,
      vitalCapacity: null,
      jump: null,
      sitAndReach: null,
      the50MeterRun: null,
      the1000MeterRun: null,
      pullUp: null,
    }
  },
  methods: {
    bindPickerChange(e: any) {
      this.index = e.detail.value;
    },

    radioChange(e: any) {
      this.current = e.detail.value;
    },

    reset(e: any) {
      console.log()
    },

    count(e: any) {
      // console.log(this.height + this.weight)
      this.$emit('outputCount', this.height)
    }
  }
}
</script>

<style scoped>
.form-container {
  padding: 16px;
  width: 100%;
  position: absolute;
  top: 200px;
}

.form-control {
  height: 60px;
  display: flex;
  align-items: center;
  justify-content: space-between;
}

/* 第一行 */
.line-1 {
  color: #859ee4;
  display: flex;
}

.grade-select {
  width: 50%;
  display: flex;
  justify-content: center;
  align-items: center;
}

.line-1 .label {
  display: inline-block;
  margin-right: 16px;
}

.grade-select>.picker {
  display: inline-block;
  border: 1px solid #dfe5f7;
}

.picker-inside {
  text-align: center;
  display: flex;
  align-items: center;
  padding:  8px;
}

.arrow{
  border-left: 1px solid #6b87e3;
  margin: 0 8px 0 16px;
}

.arrow::after {
  content: "";
  margin-left: 16px;
  display: inline-block;
  background-image: url(@/static/大于号.svg);
  background-size: cover;
  height: 8px;
  width: 8px;
  transform: rotate(90deg);
}

.gender-radio {
  width: 50%;
  display: flex;
  justify-content: center;
}

.gender-radio>.radio-group {
  display: inline-block;
}

.radio-label{
  margin-right: 16px;
}

.line-2 {
  background-color: #f6f8fb;
  font-size: 14px;
}

/* 第二行 */
.column-header {
  color: #99999b;
}

.reset-button {
  border: 1px solid #6b87e3;
  border-radius: 16px;
  color: #6b87e3;
  font-size: 14px;
  margin: 0;
}
</style>
