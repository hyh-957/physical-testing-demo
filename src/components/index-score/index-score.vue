<template>
  <view class="score-container">

    <view class="top">
      <view class="icon-area">
        <view class="icon book">
          <view class="label"></view>
          <text class="title">秘籍</text>
        </view>
      </view>
      <canvas canvas-id="myid" id="myid" class="charts" @tap="tap" />
      <view class="icon-area">
        <view class="icon service">
          <view class="label"></view>
          <text class="title">客服</text>
        </view>
        <view class="icon share">
          <view class="label"></view>
          <text class="title">分享</text>
        </view>
      </view>
      <text class="check">查看体质报告</text>
    </view>
    <view class="bottom">
      <text class="bottom-button">学校成绩查询</text>
      <text class="bottom-button">毕业成绩计算</text>
      <text class="bottom-button">国家体测要求</text>
    </view>
  </view>
</template>

<script>
import uCharts from '@qiun/ucharts';
let uChartsInstance = {};
export default {
  props: {
    score: {
      type: Number,
      required: true
    }
  },
  data() {
    return {
      cWidth: 138,
      cHeight: 138
    }
  },

  mounted() {
    this.getServerData();
  },
  watch: {
    score(newScore, oldScore) {
      this.getServerData();
    }
  },
  methods: {
    getServerData() {
      let res = {
        series: [
          {
            name: "体测分数",
            color: "#FFF",
            data: this.score / 100
          }
        ]
      };
      this.drawCharts('myid', res);
    },

    drawCharts(id, data) {
      const ctx = uni.createCanvasContext(id, this);
      uChartsInstance[id] = new uCharts({
        type: "arcbar",
        context: ctx,
        width: this.cWidth,
        height: this.cHeight,
        series: data.series,
        animation: true,
        background: "#FFFFFF",
        color: ["#1890FF", "#91CB74", "#FAC858", "#EE6666", "#73C0DE", "#3CA272", "#FC8452", "#9A60B4", "#ea7ccc"],
        padding: undefined,
        title: {
          name: "体测分数",
          fontSize: 12,
          color: "#FFF",
          offsetY: -16
        },
        subtitle: {
          name: this.score === 0 ? "0": this.score,
          fontSize: 18,
          color: "#FFF",
          offsetY: -8
        },
        extra: {
          arcbar: {
            type: "default",
            width: 8,
            backgroundColor: "#769bf6",
            startAngle: 0.75,
            endAngle: 0.25,
            gap: 2,
            // linearType: "custom"
          }
        }
      });
    },
    tap(e) {
      uChartsInstance[e.target.id].touchLegend(e);
      uChartsInstance[e.target.id].showToolTip(e);
    }
  }
}
</script>

<style scoped>
.score-container {
  height: 200px;
  width: 100%;
  background: linear-gradient(#4b92ff, rgb(89 127 255));
  padding: 16px;
  position: fixed;
  z-index: 11;
}

.top {
  height: 138px;
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.icon-area {
  display: flex;
  flex-direction: column;
}

.icon {
  display: inline-block;
  text-align: center;
  cursor: pointer;
}

.label {
  height: 40px;
  width: 40px;
  background-size: contain;
}

.book>.label {
  background-image: url(@/static/书籍.svg);
}

.service>.label {
  background-image: url(@/static/客服管理.svg);
}

.share>.label {
  background-image: url(@/static/分享.svg);
}

.title {
  font-size: 14px;
  color: #FFF;
}

.check {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translateX(-50%);
  color: #b6ccfe;
  font-size: 12px;
  margin-top: 6px;
}

.bottom {
  display: flex;
  justify-content: space-between;
  font-size: 12px;
  color: #FFF;
  background-color: rgb(103 143 255);
  padding: 8px;
}

.bottom-button {
  flex: 1;
  text-align: center;
  cursor: pointer;
}

.bottom-button:nth-child(2) {
  border-left: 1px solid #FFF;
  border-right: 1px solid #FFF;
  ;
}

.charts {
  width: 138px;
  height: 100%;
}
</style>
