<template>
  <!-- 使用scroll-view原因：微信小程序会有父元素position:fixed,子元素uchart图却跟随滚动的现象。参考：https://www.cnblogs.com/i-douya/p/9277183.html -->
  <scroll-view scroll-y='true' style="height: calc(100vh - 244px); position: absolute;top: 200px;">
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

        <view class="form-control" v-if="current === '1'">
          <text>1000米跑(秒)</text>
          <input type="number" v-model="the1000MeterRun" placeholder="请输入" />
        </view>

        <view class="form-control" v-if="current === '0'">
          <text>800米跑(秒)</text>
          <input type="number" v-model="the800MeterRun" placeholder="请输入" />
        </view>

        <view class="form-control" v-if="current === '1'">
          <text>引体向上(次)</text>
          <input type="number" v-model="pullUp" placeholder="请输入" />
        </view>

        <view class="form-control" v-if="current === '0'">
          <text>仰卧起坐(次)</text>
          <input type="number" v-model="sitUp" placeholder="请输入" />
        </view>

      </form>



    </view>
  </scroll-view>



</template>

<script>

export default {
  props: ['keyS'],
  emits: ['outputCount', 'outputSelectValueChange'],
  data() {
    return {
      array: ['大一', '大二', '大三', '大四'],
      index: 0,
      current: "1",
      height: null,//BMI=体重÷身高^2
      weight: null,
      vitalCapacity: null, //15
      jump: null, //10
      sitAndReach: null, //10
      the50MeterRun: null, //20
      the1000MeterRun: null, //20
      the800MeterRun: null,
      pullUp: null, //10,
      sitUp: null,
      sumA: [0, 0, 0, 0, 0, 0]
    }
  },
  mounted() {

  },
  onLoad() {

  },
  watch: {
    height(n, o) {
      this.sum('BMI', n, 15, 6, 'BMI');
    },
    weight(n, o) {
      this.sum('BMI', n, 15, 6, 'BMI');
    },
    vitalCapacity(n, o) {
      this.sum('vitalCapacity', n, 15, 0, 'high');
    },
    jump(n, o) {
      this.sum('jump', n, 10, 1, 'high');
    },
    sitAndReach(n, o) {
      this.sum('sitAndReach', n, 10, 2, 'high');
    },
    the50MeterRun(n, o) {
      this.sum('the50MeterRun', n, 20, 3, 'low');
    },
    the1000MeterRun(n, o) {
      this.sum('the1000MeterRun', n, 20, 4, 'low');
    },
    the800MeterRun(n, o) {
      this.sum('the800MeterRun', n, 20, 4, 'low');
    },

    pullUp(n, o) {
      this.sum('pullUp', n, 10, 5, 'high');
    },
    sitUp(n, o) {
      this.sum('sitUp', n, 10, 5, 'high');
    },
  },
  methods: {
    bindPickerChange(e) {
      this.index = e.detail.value;
      this.$emit('outputSelectValueChange', this.index, this.current);
    },

    radioChange(e) {
      this.current = e.detail.value;
      this.$emit('outputSelectValueChange', this.index, this.current);
    },

    reset(e) {
      // console.log();
      this.sumA = [0, 0, 0, 0, 0, 0];
      this.count1();
    },
    count1() {
      this.$emit('outputCount', this.sumA.reduce((a, b) => a + b))
    },

    sum(key, n, weight, index, type) {
      console.log(this.keyS.index)
      const i = this.keyS.index.indexOf(key);
      const s = this.keyS.standard[i];
      const sarray = s.standardDetal;

      let tempS = 0;
      let tempE;
      if (type === 'high') {
        sarray.some(element => {
          if (n >= element.edge) {
            tempE = element.edge;
            tempS = element.score;
            return true;
          }
        });
      } else if (type === "low") {
        sarray.some(element => {
          if (n <= element.edge) {
            tempE = element.edge;
            tempS = element.score;
            return true;
          }
        });
      } else {
        const BMI = parseInt(this.countBMI() * 10) / 10; //保留一位小数
        if (BMI > 0) {
          sarray.forEach(element => {
            element.range.forEach(element1 => {
              const e1 = element1[0] ?? -Infinity;
              const e2 = element1[1] ?? Infinity;
              if (e1 <= BMI && BMI <= e2) {
                tempS = element.score;
              }
            });
          });
        }
      }
      const temp = tempS * weight / 100;
      this.sumA[index] = temp;
      this.count1();
    },

    countBMI() {
      if (this.height && this.weight) {
        return Number(this.weight) / ((Number(this.height) / 100) ** 2);
      } else {
        return 0;
      }

    }
  }
}
</script>

<style scoped>
.form-container {
  padding: 16px;
  width: 100%;
  /* position: absolute;
  top: 200px; */
  box-sizing: border-box;
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
  padding: 8px;
}

.arrow {
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

.radio-label {
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
