<template>
  <div id="app">

    <!--3. 组件使用-->
    <TitleHeader v-bind:seller="seller"></TitleHeader>
     <!--使用 router-link 组件来导航.-->
     <!--通过传入 `to` 属性指定链接.-->
     <!--<router-link> 默认会被渲染成一个 `<a>` 标签-->
    <!--注意：<router-link>会被渲染成一个 <a> 标签，打开源码可以看到-->
    <div class="tab">
      <router-link to="/goods">商品</router-link>
      <router-link to="/ratings">评论</router-link>
      <router-link to="/seller">商家</router-link>
    </div>
    <!-- 路由出口 -->
    <!-- 路由匹配到的组件将渲染在这里 -->
    <router-view v-bind:seller="seller"></router-view>
  </div>
</template>

<script type="text/ecmascript-6">
  //  1. 引入组件
  import TitleHeader from './components/header/title_header.vue'

  const  ERR_OK = 0;  //状态碼， 返回ok

  export default {
    name: 'app',
    data() {
      return {
        seller:{}
      }
    },
    created(){
      this.$http.get('/api/seller').then((response)=>{
          console.log(response);
          response = response.body; //get json object
          if(response.errno === ERR_OK){
              this.seller = response.data;
              console.log(this.seller);
          }
      });
    },
    // 2 组件注册
    components: {TitleHeader}
  }
</script>

<style lang="scss" scoped>
  .tab{
    display: flex;
    width: 100%;
    /*height == line-height 内容垂直居中*/
    height: 40px;
    line-height: 40px;
    /*这儿将router-link看成a标签处理就ok了，因为渲染成一个 `<a>` 标签*/
    a{
      flex: 1;
      /*文字水平居中*/
      text-align: center;
      color: rgb(77,85,93);
    }
    /*点击之后变颜色*/
    a:hover{
      color: rgb(240, 20, 20);
    }
  }
</style>
