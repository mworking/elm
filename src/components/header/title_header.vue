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
              <span class="icon" v-bind:class = "classMap[seller.supports[0].type]"></span>
              <span class="text"> {{ seller.supports[0].description }} </span>
          </div>

        </div>
        <div v-if="seller.supports" class="support-count" v-on:click="showDetail">
            <span class="count">{{ seller.supports.length }} 个</span>
            <i class="icon-keyboard_arrow_right"> > </i>
        </div>
      </div>
      <div class="bulletin-wrapper" v-on:click="showDetail">
          <span class="bulletin-title"></span>
          <span class="bulletin-text">{{ seller.bulletin }}</span>
          <i class="icon-keyboard_arrow_right"> > </i>
      </div>
      <!--这个头部的背景-->
      <div class="backgroud">
          <img v-bind:src="seller.avatar" width="100%" height="100%">
      </div>
      <!--当detailsShow为false时，会转化为 style="display: none;" f12就能看见-->
      <div class="detail" v-show="detailsShow">
          <div class="detial-wrapper clearfix">
              <div class="detail-main">
                  <p>{{ seller.bulletin }}</p>
                  <p>{{ seller.bulletin }}</p>
                  <p>{{ seller.bulletin }}</p>
              </div>
          </div>
          <div class="detail-close">
              <i class="icon-close"> x </i>
          </div>
      </div>
  </div>
</template>

<script>
    export default{
        //props 获取父组件传递的内容
        props: {
            seller: {
                type: Object
            }
        },
        data(){
            return {
                detailsShow: false
            }
        },
        methods:{
          showDetail(){
              this.detailsShow = true;
          }
        },
        created(){
            this.classMap = ['decrease', 'discount', 'special', 'invoice', 'gurantee'];
        }
    }
</script>

<style lang="scss" scoped>

  .header{
      position: relative;
      /*filter:blur 会使阴影溢出到下面，所以这儿隐藏了*/
      overflow: hidden;
      color: #fff;
      /*做个透明层，模糊的效果*/
      background: rgba(7,17,27,0.5);
      .content-wrapper{
          /*因为对support-count 进行绝对定位， 父布局需要采用相对定位*/
          position: relative;
        /*上 右 下 左*/
        padding: 24px 12px 18px 24px;
        /*小技巧: avatar 和 content 是贴合在一起的，就是上下文字有距离，所以
          在他们的父步布局 设置 font-size: 0; 但是子类必须单独设置 font-size
          否则的话，子类会继承这个而显示不出字来
        */
        font-size: 0;
        /*avatar content 这个和头像是在一个行内的，所以设置为display: inline-block*/
        .avatar {
            /*右边文字与第一行顶部对齐*/
            vertical-align: top;
            display: inline-block;
            img{
                border-radius: 2px;
            }
        }
        .content{
          display: inline-block;
          margin-left: 16px;
          font-size: 14px;
          .title{
            margin:  2px 0 8px 0;
            .brand{
                /*还是无法对齐，设置对齐方式*/
                vertical-align: top;
                width: 30px;
                height: 18px;
                /*因为span是个行内元素，所以指定宽高是不生效的，需要设置为
                行内块元素*/
                display: inline-block;
                background-image: url("./brand@2x.png");
                background-size: 30px 18px;
                background-repeat:no-repeat;
            }
            .name{
                margin-left: 6px;
                font-size: 16px;
                line-height: 18px;
                font-weight: bold;
            }
          }
          .description{
              margin-bottom: 10px;
              line-height: 12px;
              font-size: 12px;
          }
          .support{
              .icon{
                  display: inline-block;
                  vertical-align: top;
                  width: 12px;
                  height: 12px;
                  margin-right: 4px;
                  background-size: 12px 12px;
                  background-repeat: no-repeat;
              }
              .decrease{
                  background-image: url("./decrease_1@2x.png");
              }
              .discount{
                  background-image: url('./discount_1@2x.png');
              }
              .gurantee{
                  background-image: url('./guarantee_1@2x.png');
              }
              .invoice{
                  background-image: url('./invoice_1@2x.png');
              }
              .special{
                  background-image: url('./special_1@2x.png');
              }
              .text{
                  line-height: 12px;
                  font-size: 10px;
              }
          }
        }
        .support-count{
            position: absolute;
            right: 12px;
            bottom: 18px;
            padding: 0 8px;
            height: 24px;
            line-height: 24px;
            border-radius: 14px;
            background: rgba(0,0,0,0.2);
            text-align: center;
            .count{
                vertical-align: top;
                font-size: 10px;
            }
            .icon-keyboard_arrow_right{
                font-size: 10px;
            }
        }
      }
      .bulletin-wrapper{
          position: relative;
          height: 28px;
          line-height: 28px;
          /*父元素设置font-size 消除空白间隙，可以看到间隙没了，图片和文字靠在一起*/
          /*font-size: 0;*/
          padding: 0 22px 0 12px;
          background: rgba(7,17,27,0.2);
          /*设置单行显示， 多余部分隐藏， 多出内容 变为点点点*/
          white-space: nowrap;
          overflow: hidden;
          text-overflow: ellipsis;
          .bulletin-title{
              display: inline-block;
              vertical-align: top;
              margin-top: 8px;
              width: 22px;
              height: 12px;
              background-image: url("./bulletin@2x.png");
              background-size: 22px 12px;
              background-repeat: no-repeat;
          }
          .bulletin-text{
              vertical-align: top;
              margin: 0 4px;
              font-size: 10px;
          }
          .icon-keyboard_arrow_right{
             /*这儿使用绝对定位， 前提是先给父元素，设置为相对定位*/
              position: absolute;
              font-size: 10px;
              right: 12px;
          }
      }
      .backgroud{
          position: absolute;
          top:0;
          left:0;
          width: 100%;
          height: 100%;
          z-index: -1;
          filter:blur(10px);
      }
      .detail{
          position: fixed;
          z-index: 100;
          top:0;
          left: 0;
          width: 100%;
          height: 100%;
          overflow: auto;
          background: rgba(7,17,27,0.8);
          .detial-wrapper{
              min-height: 100%;
              .detail-main{
                  margin-top: 64px;
                  padding-bottom: 64px;
              }
          }
          .detail-close{
              position: relative;
              width: 32px;
              height: 32px;
              margin: -64px auto 0 auto;
              /*清除浮动*/
              clear: both;
              font-size: 32px;
          }
      }
  }

</style>
















