<template>
    <div class="shopcart">
      <div class="content">
        <div class="content-left">
          <div class="logo-wrapper">
            <div class="logo">
              <i class="icon-shopping_cart"></i>
            </div>
            <div class="num">{{totalCount}}</div>
          </div>
          <div class="price">￥{{totalPrice}}</div>
          <div class="desc">另需配送费￥{{deliveryPrice}}</div>
        </div>
        <div class="content-right">
          <div class="pay" v-bind:class="payClass">
            {{payDesc}}
          </div>
        </div>
      </div>
    </div>
</template>

<script type="text/ecmascript-6">

    export default {
      props:{
        selectFoods:{
          type: Array,
          default(){
            return [
              {
                price: 30,
                count: 1
              }
            ];
          }
        },
        deliveryPrice: {
          type: Number,
          default:0
        },
        minPrice:{
          type:Number,
          default:0
        }
      },
      computed:{
        totalPrice(){
          let total = 0;
          this.selectFoods.forEach((food)=>{
            total += food.price * food.count;
          });
          return total;
        },
        totalCount(){
          let count = 0;
          this.selectFoods.forEach((food)=>{
            count += food.count;
          });
          return count;
        },
        payDesc(){
          // 这儿的 this.totalPrice 不可以写成 this.totalPrice() 会报错
          if(this.totalPrice === 0){
            return `￥${this.minPrice} 元起送`;
          }else if(this.totalPrice < this.minPrice){
            let diff = this.minPrice - this.totalPrice;
            return `还差￥${diff}元起送`;
          }else{
            return '去结算';
          }
        },
        payClass(){
          if(this.totalPrice < this.minPrice){
            return 'not-enough';
          }else{
            return 'enough';
          }
        }
      }
    }

</script>

<style lang="scss" scoped>
  .shopcart{
    position: fixed;
    left: 0;
    bottom: 0;
    z-index: 50;
    width: 100%;
    height: 48px;
    .content{
      display: flex;
      background: #141d27;
      font-size:0;
      .content-left{
        flex: 1;
        .logo-wrapper{
          display: inline-block;
          position: relative;
          top:-10px;
          margin: 0 12px;
          padding: 6px;
          width: 56px;
          height: 56px;
          /*这儿注意*/
          box-sizing: border-box;
          vertical-align: top;
          border-radius: 50%;
          background: #141d27;
          .logo{
            width: 100%;
            height: 100%;
            /*icon-shopping_cart 是把图片变成字体的东西，设置居中，在父布局中设*/
            text-align: center;
            border-radius: 50%;
            background: #2b343c;
            .icon-shopping_cart{
              font-size: 24px;
              line-height: 44px;
              color: #80858a;
            }
          }
          .highlight{
            background: rgb(0,160,220);
          }
          .num{
            position: absolute;
            top: 0;
            right: 0;
            width: 24px;
            /*如下代码使 垂直居中*/
            height: 16px;
            line-height: 16px;
            /*水平居中*/
            text-align: center;
            border-radius: 16px;
            font-size: 9px;
            font-weight: 700;
            color: #fff;
            background: rgb(240,20,20);
            box-shadow: 0 4px 8px 0 rgba(0,0,0,0.4);
          }
        }
        .price{
          display: inline-block;
          vertical-align: top;
          line-height: 24px;
          margin-top: 12px;
          box-sizing: border-box;
          padding-right: 12px;
          border-right: 1px solid rgba(255,255,255,0.1);
          font-size: 16px;
          font-weight: 700;
          color: rgba(255,255,255,0.4);
        }
        .desc{
          display: inline-block;
          vertical-align: top;
          line-height: 24px;
          margin: 12px 0 0 12px;
          color: rgba(255,255,255,0.4);
          font-size: 10px;
        }
      }
      .content-right{
        flex: 0 0 105px;
        width: 105px;
        .pay{
          height: 48px;
          line-height: 48px;
          text-align: center;
          font-size: 12px;
          color:rgba(255,255,255,0.4);
          font-weight: 700;
          background: #2b333b;
        }
        .not-enough{
          background: #2b333b;
        }
        .enough{
          background: #00b43c;
          color: #fff;
        }
      }
    }
  }
</style>

