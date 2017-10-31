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
    <div class="foods-wrapper"></div>
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
           border-bottom: 1px;
           border-bottom-color: rgba(7,17,27,0.1);
         }
       }
     }
     .foods-wrapper{
       flex: 1;
     }
   }
</style>
