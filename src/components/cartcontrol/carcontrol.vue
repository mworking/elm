<template>
  <div class="cartcontrol">
    <div class="cart-decrease icon-remove_circle_outline" v-on:click="decreaseCart" v-show="food.count>0"></div>
    <div class="cart-count" v-show="food.count>0">{{food.count}}</div>
    <div class="cart-add icon-add_circle" v-on:click="addCart"></div>
  </div>
</template>

<script type="text/ecmascript-6">

  import Vue from 'vue';

  export default {
    props: {
      food:{
        type: Object
      }
    },
    created(){
//      console.log(this.food);
    },
    methods:{
      addCart(event){
//      为了防止， H5 切换到 pc的时候，点击，一下子触发很多次
        if(!event._constructed){
          return;
        }
        console.log("click");
        if(!this.food.count){
//          这种方式无效，应为我们 food 中没有count 这个字段，所以无效
//           如果我们新增一个字段，并且希望dom来刷新的话， 需要使用vue.set 来做
//          this.food.count = 1;
          Vue.set(this.food,'count',1);
        }else{
          this.food.count++;
        }
      },
      decreaseCart(event){
        if(!event._constructed){
          return;
        }
        if(this.food.count){
          this.food.count--;
        }
      }
    }
  };

</script>

<style lang="scss" scoped>

  @import "../../common/common-css/style.css";

  .cartcontrol{
    font-size: 0;
    .cart-decrease{
      display: inline-block;
      padding: 6px;
      line-height: 24px;
      font-size: 24px;
      color: rgb(0,160,220);
    }
    .cart-count{
      display: inline-block;
      vertical-align: top;
      width: 12px;
      padding-top: 6px;
      line-height: 24px;
      text-align: center;
      font-size: 10px;
      color: rbg(147,153,159);
    }
    .cart-add{
      display: inline-block;
      line-height: 24px;
      font-size: 24px;
      color: rgb(0,160,220);
    }
  }
</style>

