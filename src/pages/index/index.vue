<template>
  <view>
    <index-score :score="score"></index-score>
    <index-form @outputCount="count"></index-form>
  </view>
</template>

<script lang="ts">
import Vue from 'vue';
export default Vue.extend({
  data() {
    return {
      score: 80
    }
  },
  methods: {
    count(e: number) {
      this.score = Number(e);
    },
  }
});
</script>

<style>


</style>
