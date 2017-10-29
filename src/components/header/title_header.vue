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

<style lang="scss" scoped>

  .header{
      color: #fff;
      background: #000;
      .content-wrapper{
        /*上 右 下 左*/
        padding: 24px 12px 18px 24px;
        /*小技巧: avatar 和 content 是贴合在一起的，就是上下文字有距离，所以
          在他们的父步布局 设置 font-size: 0; 但是子类必须单独设置 font-size
          否则的话，子类会继承这个而显示不出字来
        */
        font-size: 0;
        /*avatar content 这个和头像是在一个行内的，所以设置为display: inline-block*/
        .avatar {
          display: inline-block;
        }
        .content{
          display: inline-block;
          margin-left: 16px;
          font-size: 14px;
          .title{
            margin:  2px 0 8px 0;
            .brand{
              width: 30px;
              height: 18px;
              /*因为span是个行内元素，所以指定宽高是不生效的，需要设置为
                行内块元素*/
              display: inline-block;

            }
          }
        }
      }
  }

</style>
















