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

<script lang="ts">
import Vue from 'vue';
import uCharts from '@qiun/ucharts';
let uChartsInstance: any = {};
export default Vue.extend({
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

    drawCharts(id: string, data: any) {
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
          name: this.score === 0 ? "0" : this.score,
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
    tap(e: any) {
      uChartsInstance[e.target.id].touchLegend(e);
      uChartsInstance[e.target.id].showToolTip(e);
    }
  }
})
</script>

<style scoped>
.score-container {
  height: 200px;
  width: 100%;
  background: linear-gradient(#4b92ff, rgb(89 127 255));
  padding: 16px;
  position: fixed;
  z-index: 11;
  box-sizing: border-box;
  /* 微信小程序没有应App.vue的公共样式 */
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
  /* 书籍 */
  background-image: url(data:image/svg+xml;base64,PD94bWwgdmVyc2lvbj0iMS4wIiBzdGFuZGFsb25lPSJubyI/PjwhRE9DVFlQRSBzdmcgUFVCTElDICItLy9XM0MvL0RURCBTVkcgMS4xLy9FTiIgImh0dHA6Ly93d3cudzMub3JnL0dyYXBoaWNzL1NWRy8xLjEvRFREL3N2ZzExLmR0ZCI+PHN2ZyB0PSIxNjY5Mjc1MDk2NzMyIiBjbGFzcz0iaWNvbiIgdmlld0JveD0iMCAwIDEwMjQgMTAyNCIgdmVyc2lvbj0iMS4xIiB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHAtaWQ9IjIyMjciIHhtbG5zOnhsaW5rPSJodHRwOi8vd3d3LnczLm9yZy8xOTk5L3hsaW5rIiB3aWR0aD0iMjAwIiBoZWlnaHQ9IjIwMCI+PHBhdGggZD0iTTUwOS4xMzI4IDUxMy4xMjY0bS00NTAuODE2IDBhNDUwLjgxNiA0NTAuODE2IDAgMSAwIDkwMS42MzIgMCA0NTAuODE2IDQ1MC44MTYgMCAxIDAtOTAxLjYzMiAwWiIgZmlsbD0iIzZjOWJmZSIgcC1pZD0iMjIyOCI+PC9wYXRoPjxwYXRoIGQ9Ik0zNzIuNjg0OCAyMzkuNDYyNEgyODUuMjg2NGMtMTYuMTc5MiAwLTI5LjI4NjQgMTMuMTA3Mi0yOS4yODY0IDI5LjI4NjR2NDc4LjQ2NGMwIDE2LjE3OTIgMTMuMTA3MiAyOS4yODY0IDI5LjI4NjQgMjkuMjg2NGg4Ny4zOTg0YzE2LjE3OTIgMCAyOS4yODY0LTEzLjEwNzIgMjkuMjg2NC0yOS4yODY0VjI2OC43NDg4YzAtMTYuMTI4LTEzLjEwNzItMjkuMjg2NC0yOS4yODY0LTI5LjI4NjR6IG0tNDMuNjczNiA1MDIuMzc0NGEyNy45NTUyIDI3Ljk1NTIgMCAxIDEgMC4wMDUxMi01NS45MTU1MiAyNy45NTUyIDI3Ljk1NTIgMCAwIDEtMC4wMDUxMiA1NS45MTU1MnpNNzk0LjIxNDQgNzIxLjEwMDhsLTczLjExMzYtNDMwLjM4NzJhMjkuMjk2NjQgMjkuMjk2NjQgMCAwIDAtMzMuNzkyLTIzLjk2MTZsLTg2LjE2OTYgMTQuNjQzMmEyOS4xODQgMjkuMTg0IDAgMCAwLTIzLjM5ODQgMjEuNDUyOFYyNjguOGMwLTE2LjE3OTItMTMuMTA3Mi0yOS4yODY0LTI5LjI4NjQtMjkuMjg2NEg0NjEuMTA3MmMtMTYuMTc5MiAwLTI5LjI4NjQgMTMuMTA3Mi0yOS4yODY0IDI5LjI4NjR2NDc4LjQ2NGMwIDE2LjE3OTIgMTMuMTA3MiAyOS4yODY0IDI5LjI4NjQgMjkuMjg2NGg4Ny4zOTg0YzE2LjE3OTIgMCAyOS4yODY0LTEzLjEwNzIgMjkuMjg2NC0yOS4yODY0VjMxOC40NjRsNzIuNTUwNCA0MjcuMDU5MmEyOS4yOTY2NCAyOS4yOTY2NCAwIDAgMCAzMy43OTIgMjMuOTYxNmw4Ni4xNjk2LTE0LjY0MzJjMTUuOTIzMi0yLjY2MjQgMjYuNjI0LTE3LjgxNzYgMjMuOTEwNC0zMy43NDA4eiBtLTI4OS40MzM2IDIwLjczNmEyNy45NTUyIDI3Ljk1NTIgMCAxIDEgMC4wMDUxMi01NS45MTU1MiAyNy45NTUyIDI3Ljk1NTIgMCAwIDEtMC4wMDUxMiA1NS45MTU1MnogbTIxNi42Nzg0LTEzLjI2MDhhMjguNTE4NCAyOC41MTg0IDAgMCAxLTMyLjg3MDQtMjMuMjk2IDI4LjUxODQgMjguNTE4NCAwIDAgMSAyMy4yOTYtMzIuODcwNCAyOC41MTg0IDI4LjUxODQgMCAwIDEgMzIuODcwNCAyMy4yOTYgMjguNDI2MjQgMjguNDI2MjQgMCAwIDEtMjMuMjk2IDMyLjg3MDR6IiBmaWxsPSIjRkZGRkZGIiBwLWlkPSIyMjI5Ij48L3BhdGg+PC9zdmc+);
}

.service>.label {
  /* 客服管理 */
  background-image: url(data:image/svg+xml;base64,PD94bWwgdmVyc2lvbj0iMS4wIiBzdGFuZGFsb25lPSJubyI/PjwhRE9DVFlQRSBzdmcgUFVCTElDICItLy9XM0MvL0RURCBTVkcgMS4xLy9FTiIgImh0dHA6Ly93d3cudzMub3JnL0dyYXBoaWNzL1NWRy8xLjEvRFREL3N2ZzExLmR0ZCI+PHN2ZyB0PSIxNjY5Mjc1MDQzNzI1IiBjbGFzcz0iaWNvbiIgdmlld0JveD0iMCAwIDEwMjQgMTAyNCIgdmVyc2lvbj0iMS4xIiB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHAtaWQ9Ijc3MCIgeG1sbnM6eGxpbms9Imh0dHA6Ly93d3cudzMub3JnLzE5OTkveGxpbmsiIHdpZHRoPSIyMDAiIGhlaWdodD0iMjAwIj48cGF0aCBkPSJNNTA5LjkwMDggNTA5LjU5MzZtLTQ1MC44MTYgMGE0NTAuODE2IDQ1MC44MTYgMCAxIDAgOTAxLjYzMiAwIDQ1MC44MTYgNDUwLjgxNiAwIDEgMC05MDEuNjMyIDBaIiBmaWxsPSIjNmM5YmZlIiBwLWlkPSI3NzEiPjwvcGF0aD48cGF0aCBkPSJNNzY0LjMxMzYgNDIwLjQ1NDRjLTguNjAxNi0xMjcuMzM0NC0xMTcuMDk0NC0yMjguMzUyLTI0OS4xOTA0LTIyOC4zNTItMTMxLjU4NCAwLTIzOS42MTYgMTAwLjE0NzItMjQ5LjA4OCAyMjYuNzEzNi0zNS4zNzkyIDE1LjEwNC02MC4yNjI0IDUwLjIyNzItNjAuMjYyNCA5MS4wODQ4djEwMS41ODA4YzAgNTQuNTc5MiA0NC4zOTA0IDk4Ljk2OTYgOTguOTY5NiA5OC45Njk2aDM4LjE0NGMxOS44MTQ0IDAgMzUuODQtMTYuMDI1NiAzNS44NC0zNS44NHYtMjI3Ljg0YzAtMTkuODE0NC0xNi4wMjU2LTM1Ljg0LTM1Ljg0LTM1Ljg0aC0zLjk0MjRjMTIuOTAyNC04My4xNDg4IDg2Ljk4ODgtMTQ3LjE0ODggMTc2LjEyOC0xNDcuMTQ4OCA4OS4xOTA0IDAgMTYzLjIyNTYgNjMuOTQ4OCAxNzYuMTc5MiAxNDcuMTQ4OGgtNy4yMTkyYy0xOS44MTQ0IDAtMzUuODQgMTYuMDI1Ni0zNS44NCAzNS44NHYyMjcuODRjMCAxMi45NTM2IDYuOTEyIDI0LjI2ODggMTcuMjAzMiAzMC41NjY0LTIzLjM5ODQgMjIuNzMyOC02MC42MjA4IDQ4LjY5MTItMTEzLjIwMzIgNTYuMTE1MmEzNy45MzQwOCAzNy45MzQwOCAwIDAgMC0yMi42ODE2LTcuNTI2NGgtMjYuMTEyYTM4LjE5NTIgMzguMTk1MiAwIDAgMCAwIDc2LjM5MDRoMjYuMTEyYzExLjMxNTIgMCAyMS40NTI4LTQuOTY2NCAyOC40NjcyLTEyLjggOTkuOTQyNC0xMy4wNTYgMTU2LjkyOC03OC40ODk2IDE3Ny44MTc2LTEwNy45ODA4IDQ4LjEyOC02LjcwNzIgODUuMjk5Mi00Ny45NzQ0IDg1LjI5OTItOTcuOTQ1NlY1MDkuOTAwOGMwLjEwMjQtMzkuNDc1Mi0yMy4yNDQ4LTczLjU3NDQtNTYuNzgwOC04OS40NDY0eiIgZmlsbD0iI0ZGRkZGRiIgcC1pZD0iNzcyIj48L3BhdGg+PC9zdmc+);
}

.share>.label {
  /* 分享 */
  background-image: url(data:image/svg+xml;base64,PD94bWwgdmVyc2lvbj0iMS4wIiBzdGFuZGFsb25lPSJubyI/PjwhRE9DVFlQRSBzdmcgUFVCTElDICItLy9XM0MvL0RURCBTVkcgMS4xLy9FTiIgImh0dHA6Ly93d3cudzMub3JnL0dyYXBoaWNzL1NWRy8xLjEvRFREL3N2ZzExLmR0ZCI+PHN2ZyB0PSIxNjY5Mjc1MDkxMTA2IiBjbGFzcz0iaWNvbiIgdmlld0JveD0iMCAwIDEwMjQgMTAyNCIgdmVyc2lvbj0iMS4xIiB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHAtaWQ9IjIwNzIiIHhtbG5zOnhsaW5rPSJodHRwOi8vd3d3LnczLm9yZy8xOTk5L3hsaW5rIiB3aWR0aD0iMjAwIiBoZWlnaHQ9IjIwMCI+PHBhdGggZD0iTTUwNC4zNzEyIDUwOS41OTM2bS00NTAuODE2IDBhNDUwLjgxNiA0NTAuODE2IDAgMSAwIDkwMS42MzIgMCA0NTAuODE2IDQ1MC44MTYgMCAxIDAtOTAxLjYzMiAwWiIgZmlsbD0iIzZjOWJmZSIgcC1pZD0iMjA3MyI+PC9wYXRoPjxwYXRoIGQ9Ik0xNDMuMDUyOCA1ODQuODU3NmwxODMuODA4IDY5LjA2ODhhMTEuMjY0IDExLjI2NCAwIDAgMCAxMS42MjI0LTIuMzA0bDI3OS43NTY4LTI2MS40MjcyYzAuOTIxNi0wLjg3MDQgMi4zNTUyIDAuNDA5NiAxLjUzNiAxLjQzMzZsLTIxMy44NjI0IDI3Mi4wNzY4Yy00LjY1OTIgNS45MzkyLTIuMjUyOCAxNC42OTQ0IDQuODEyOCAxNy40MDhsMjMxLjk4NzIgODguODgzMmM2LjU1MzYgMi41MDg4IDEzLjcyMTYtMS41MzYgMTUuMDUyOC04LjM5NjhsMTAyLjI5NzYtNTM2LjcyOTZjMS43OTItOS40NzItOC4zNDU2LTE2LjY0LTE2LjY5MTItMTEuNzc2TDE0MS4zMTIgNTY0LjY4NDhjLTguMTQwOCA0Ljc2MTYtNy4xMTY4IDE2Ljg0NDggMS43NDA4IDIwLjE3Mjh6TTM5OS4yNTc2IDcxNC43MDA4bDU0LjQ3NjggMjMuNzU2OGM3LjMyMTYgMy4xNzQ0IDkuMDYyNCAxMi44NTEyIDMuMjc2OCAxOC4zODA4bC01NC40NzY4IDUyLjQyODhjLTcuMTE2OCA2Ljg2MDgtMTguOTk1MiAxLjc5Mi0xOC45OTUyLTguMDg5NlY3MjQuOTkyYzAtOC4wODk2IDguMjk0NC0xMy41MTY4IDE1LjcxODQtMTAuMjkxMnoiIGZpbGw9IiNGRkZGRkYiIHAtaWQ9IjIwNzQiPjwvcGF0aD48L3N2Zz4=);
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
