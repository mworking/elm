<template>
  <div class="header">
      <div class="content-wrapper">
        <div class="avatar">
          <img v-bind:src="seller.avatar" width="64px" height="64px">
        </div>
      </div>
      <div class="bulletin-wrapper"></div>
  </div>
</template>

<script>
 export default{
//props 获取父组件传递的内容
   props:{
     seller:{
       type: Object
     }
   }
 }
</script>

<style>

</style>
