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
      score: 0,
      keyS: {}
    }
  },
  onLoad() {
	
    // console.log(d)
		// app会报错，所以用fail
    uni.request({
      url: '../../static/config.json',
      success: (res) => {
        this.keyS = (res.data as AnyObject).key[0];
        console.log(this.keyS)
      },
			fail:(res)=> {
				this.keyS = d.key[0];
			}
    });

  },
  methods: {
    count(e: number) {
      // console.log(e)
      this.score = Number(e);

    },
  }
});
</script>

<style>

</style>
