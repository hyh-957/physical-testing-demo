<template>
  <view>
    <index-score :score="score"></index-score>
    <index-form :keyS=keyS @outputCount="count" @outputSelectValueChange="selectValueChange"></index-form>
  </view>
</template>

<script lang="ts">
import Vue from 'vue';
import * as d from '../../static/config.json'
export default Vue.extend({
  data() {
    return {
      score: 0,
      keyS: {},
      rawKeyS: [] as any[]
    }
  },
  onLoad() {

    // console.log(d)
    // app会报错，所以用fail
    uni.request({
      url: '../../static/config.json',
      success: (res) => {
        this.rawKeyS = (res.data as AnyObject).key;
        this.keyS = (res.data as AnyObject).key[0];
        // console.log(this.keyS)
      },
      fail: (res) => {
        this.rawKeyS = d.key;
        this.keyS = d.key[0];
      }
    });

  },
  methods: {
    count(e: number) {
      // console.log(e)
      this.score = Number(e);

    },

    selectValueChange(val1: 1 | 2 | 3 | 4, val2: "1" | "0") {
      // console.log(val1);
      // console.log(val2);
      if (val1 === 1 || val1 === 2) { //大一、大二
        if (val2 === "1") { // 男
          this.keyS = this.rawKeyS[0];
        } else { // 女
          this.keyS = this.rawKeyS[2];
        }
      } else { //大三、大四
        if (val2 === "1") {
          this.keyS = this.rawKeyS[1];
        } else {
          this.keyS = this.rawKeyS[3];
        }
      }
    }
  }
});
</script>

<style>

</style>
