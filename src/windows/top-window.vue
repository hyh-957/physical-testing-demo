<template>
  <view class="header">
    <text class="title">{{title}}</text>
  </view>
</template>

<script lang='ts'>
export default {
  data() {
    return {
      title: '体测小帮手'
    }
  },
}

</script>

<style scoped>
.header{
  background-color: #4b92ff;
  height: 100%;
  padding: 0 16px;
  display: flex;
  flex-direction: column;
  justify-content: center;
}

.title{
  color: #FFF;
  letter-spacing: 2px;
}
</style>
