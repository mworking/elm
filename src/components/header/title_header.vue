<template>
  <div class="header">
      <div class="content-wrapper">
        <!--左侧头像区域-->
        <div class="avatar">
          <img v-bind:src="seller.avatar" width="64px" height="64px">
        </div>
        <!--右侧内容区域-->
        <div class="content">
           <div class="title">
             <span class="brand"></span>
             <span class="name"> {{ seller.name }} </span>
           </div>
          <div class="description">
            {{ seller.description }} / {{ seller.deliveryTime }}
          </div>
          <div v-if="seller.supports" class="support">
              <span class="icon"></span>
              <span class="text"> {{ seller.supports[0].description }} </span>
          </div>

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

<style >

</style>
