<template>
  <div class="ratings" ref="ratings">
    <div class="ratings-content">
      <div class="overview">
        <div class="overview-left">
          <h1 class="score">{{seller.score}}</h1>
          <div class="title">综合评分</div>
          <div class="rank">高于周边商家{{seller.rankRate}}%</div>
        </div>
        <div class="overview-right">
          <div class="score-wrapper">
            <span>服务态度</span>
            <star :size="36" :score="seller.serviceScore"></star>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script type="text/ecmascript-6">

  import BScroll from 'better-scroll';
  import {formateDate} from '../../common/js/date'
  import star from '../../components/star/star'
  import ratingselect from '../../components/ratingselect/ratingselect'

  export default{
    props:{
      // 通过这种方式拿到 App.vue 中传递过来的 seller <router-view v-bind:seller="seller"></router-view>
      seller:{
        type:Object
      }
    },
    //组件注册，不注册使用会报错
    components:{
      star
    }
  };

</script>

<style>

</style>
