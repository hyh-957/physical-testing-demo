<template>
  <view>
    <index-score :score="score"></index-score>
    <index-form :keyS=keyS @outputCount="count"></index-form>
  </view>
</template>

<script lang="ts">
import Vue from 'vue';
import * as d from '../../static/config.json'
export default Vue.extend({
  data() {
    return {
      score: 80,
      keyS: {}
    }
  },
  onLoad() {

    // console.log(d)
    uni.request({
      url: '../../static/config.json',
      success: (res) => {
        this.keyS = (res.data as AnyObject).key[0];
        console.log(this.keyS)
      }
    });

  },
  methods: {
    count(e: number) {
      console.log(e)
      this.score = Number(e);

    },
  }
});
</script>

<style>

</style>
