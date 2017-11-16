<template>
  <div class="goods">
    <div class="menu-wrapper" ref="menuWrapper">
      <ul>
        <li v-for="item,index in goods" class="menu-item" v-bind:class="{'current' : currentIndex === index}" v-on:click="selectMenu(index,$event)">
          <span class="text">
            <span v-show="item.type>0" class="icon" v-bind:class="classMap[item.type]"></span>
            {{ item.name }}
          </span>
        </li>
      </ul>
    </div>
    <div class="foods-wrapper" ref="foodsWrapper">
      <ul>
        <!--food-list-hook 写这个样式，只是为了让js来选择-->
        <li v-for="item in goods" class="food-list food-list-hook">
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
                  <span class="now">￥{{food.price}}</span>
                  <span class="old" v-show="food.oldPrice">￥{{food.oldprice}}</span>
                </div>
                <div class="cartcontrol-wrapper">
                  <carcontrol v-bind:food="food"></carcontrol>
                </div>
              </div>
            </li>
          </ul>
        </li>
      </ul>
    </div>
    <shopcart v-bind:delivery-price="seller.deliveryPrice" v-bind:min-price="seller.minPrice"></shopcart>
  </div>
</template>

<script type="text/ecmascript-6">

  import BScroll from 'better-scroll';
  import shopcart from '../shopcart/shopCart.vue';
  import carcontrol from '../cartcontrol/carcontrol.vue';

  const ERR_OK = 0;

  export default{
    data() {
      return {
        goods:[],
        listHeight:[],
        scrollY:0
      }
    },
    props:{
      seller:{
        type: Object
      }
    },
    computed:{
      //当前我这个索引在哪儿
      currentIndex() {
          for(let i =0; i<this.listHeight.length; i++){
            let height1 = this.listHeight[i];
            let height2 = this.listHeight[i + 1];
            if(!height2 || (this.scrolly >= height1 && this.scrolly < height2)){
              return i;
            }
          }
          return 0;
      }
    },
    created(){
      this.classMap = ['decrease', 'discount', 'special', 'invoice', 'gurantee'];
      this.$http.get('/api/goods').then((response) => {
        response = response.body;
        if(response.errno === ERR_OK){
          this.goods = response.data;
          //进行一些DOM相关的操作的时候，我们需要保证这个DOM已经渲染了，写在这儿
          //虽然vue是数据变了， dom就变了，但真正变化，是在这个函数后
          this.$nextTick(() => {
            this._initScroll();
            this._calculateHeight();
          });
        }
      });
    },
    methods: {
      selectMenu(index,event){
        //就是当我们向浏览器派发一个点击事件的时候，浏览器也会派发一个点击事件，
        //这样方法就会执行两次，这儿做个判断，当我们派发事件的时候event._constructed 这个东西返回true；
        //就是浏览器是手机模式的时候，该方法执行一次，如果不是手机模式， 该方法执行两次
        if(!event._constructed){
          return;
        }
        let foodList = this.$refs.foodsWrapper.getElementsByClassName('food-list-hook');
        let el = foodList[index];
        this.foodsScroll.scrollToElement(el,300);
      },
      _initScroll(){
        this.menuScroll = new BScroll(this.$refs.menuWrapper,{
          click: true
        });
        this.foodsScroll = new BScroll(this.$refs.foodsWrapper,{
          //为了让列表中的按钮可以点击
          click:true,
          probeType: 3
        })
        this.foodsScroll.on('scroll',(pos) => {
          this.scrollY = Math.abs(Math.round(pos.y));
        });
      },
      _calculateHeight(){
        let foodList = this.$refs.foodsWrapper.getElementsByClassName('food-list-hook');
        let height = 0;
        this.listHeight.push(height);
        for(let i = 0; i< foodList.length;i++){
          let item = foodList[i];
          height += item.clientHeight;
          this.listHeight.push(height);
        }
      }
    },
    components:{
      shopcart,
      carcontrol
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
       .current{
         position: relative;
         margin-top: -1px;
         z-index: 10;
         background: #fff;
         font-weight: 700;
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
           /*position: relative;*/
           flex:1;
           .name{
             margin: 2px 0 8px 0;
             height: 14px;
             line-height: 14px;
             font-size: 14px;
             color: rgb(7, 17, 27);
           }
           .desc, .extra{
             line-height: 10px;
             font-size: 10px;
             color: rgb(147,153,159);
           }
           .desc{
             line-height: 12px;
             margin-bottom: 8px;
           }
           .extra{
             .count{
               margin-right: 12px;
             }
           }
           .price{
             font-weight: 700;
             line-height: 24px;
             .now{
               margin-right: 18px;
               font-size: 14px;
               color: rgb(240,20,20);
             }
             .old{
               text-decoration: line-through;
               font-size: 10px;
               color: rgb(147,153,159);
             }
           }
           .cartcontrol-wrapper{
             /*这儿先不设置了，这儿要有效果，需要将父布局，.content设置属性 position： relative*/
             /*position: absolute;*/
             /*bottom: 12px;*/
             /*right: 0px;*/
           }
         }
       }
       .food-item:last-child{
         margin-bottom: 0;
       }
     }
   }
</style>
