<template>
  <div class="goods">
    <div class="menu-wrapper">
      <ul>
        <li v-for="item,index in goods" class="menu-item">
          <span class="text">
            <span v-show="item.type>0" class="icon" v-bind:class="classMap[item.type]"></span>
            {{ item.name }}
          </span>
        </li>
      </ul>
    </div>
    <div class="foods-wrapper">
      <ul>
        <li v-for="item in goods" class="food-list">
          <h1 class="title">{{ item.name }}</h1>
          <ul>
            <li v-for="food in item.foods" class="food-item">
              <div class="icon">
                <img v-bind:src="food.icon" height="57px">
              </div>
              <div class="content">
                <h2 class="name">{{food.name}}</h2>
                <p class="desc">{{food.description}}</p>
                <div class="extra">
                  <span class="count">月售{{food.sellCount}}份</span>
                  <span>好评率{{food.rating}}%</span>
                </div>
                <div class="price">
                  <span>￥{{food.price}}</span>
                  <span v-show="food.oldPrice">￥{{food.oldprice}}</span>
                </div>
              </div>
            </li>
          </ul>
        </li>
      </ul>
    </div>
  </div>
</template>

<script type="text/ecmascript-6">

  const ERR_OK = 0;

  export default{
    data() {
      return {
        goods:[]
      }
    },
    props:{
      seller:{
        type: Object
      }
    },
    created(){
      this.classMap = ['decrease', 'discount', 'special', 'invoice', 'gurantee'];
      this.$http.get('/api/goods').then((response) => {
        response = response.body;
        if(response.errno === ERR_OK){
          this.goods = response.data;
        }
      });
    }
  }
</script>

<style lang="scss" scoped>
   .goods{
     display: flex;
     position: absolute;
     top:174px;
     bottom: 46px;
     width: 100%;
     overflow: hidden;
     .menu-wrapper{
       flex: 0 0 80px;
       width: 80px;
       background: #f3f5f7;
        /*尽量使用class，标签的效率是比较低的，尤其是层级很深的时候，效率会很受影响*/
       .menu-item{
         /*垂直居中，多行 小技巧， display：table  不管一行还是两行，都可以做到垂直居中*/
         display: table;
         height: 54px;
         width: 56px;
         padding: 0 12px;
         line-height: 14px;
         .icon{
           display: inline-block;
           vertical-align: top;
           width: 12px;
           height: 12px;
           margin-right: 2px;
           background-size: 12px 12px;
           background-repeat: no-repeat;
         }
         .decrease{
           background-image: url("./decrease_3@2x.png");
         }
         .discount{
           background-image: url('./discount_3@2x.png');
         }
         .gurantee{
           background-image: url('./guarantee_3@2x.png');
         }
         .invoice{
           background-image: url('./invoice_3@2x.png');
         }
         .special{
           background-image: url('./special_3@2x.png');
         }
         .text{
           /*table 布局，对应前面的 menu-item display: table*/
           display: table-cell;
           width: 56px;
           vertical-align: middle;
           font-size: 12px;
           border-bottom: 1px solid;
           border-bottom-color: rgba(7,17,27,0.1);
         }
       }
     }
     .foods-wrapper{
       flex: 1;
       .title{
         padding-left: 14px;
         height: 26px;
         line-height: 26px;
         border-left: 2px solid #d9dde1;
         font-size: 12px;
         color: rgb(147,153,159);
         background: #f3f5f7;
       }
       .food-item{
         display: flex;
         margin: 18px;
         border-bottom: 1px solid;
         border-bottom-color: rgba(7,17,27,0.1);
         padding-bottom: 18px;
         .icon{
           flex: 0 0 57px;
           margin-right: 10px;
         }
         .content{
           flex:1;
           .name{
             margin: 2px 0 8px 0;
             height: 14px;
             line-height: 14px;
             font-size: 14px;
             color: rgb(7, 17, 27);
           }
           .desc,.extra{
             line-height: 10px;
             font-size: 10px;
             color: rgb(147,153,159);
           }
           .desc{
             margin-bottom: 8px;
           }
           .extra{
             .count{
               margin-right: 12px;
             }
           }
         }
       }
       .food-item:last-child{
         margin-bottom: 0;
       }
     }
   }
</style>
