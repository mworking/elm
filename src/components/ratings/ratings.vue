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
            <span class="score">{{seller.serviceScore}}</span>
          </div>
          <div class="score-wrapper">
            <span>商品评分</span>
            <star :size="36" :score="seller.serviceScore"></star>
            <span class="score">{{seller.foodsScore}}</span>
          </div>
          <div class="score-wrapper">
            <span>送达时间</span>
            <star :size="36" :score="seller.serviceScore"></star>
            <span class="score">{{seller.deliveryTime}} 分钟</span>
          </div>
        </div>
      </div>
      <!--绘制中间的阴影线条-->
      <split></split>
      <!--绘制全部，满意，不满意 3个tab-->
      <ratingselect @select="selectRating" @toggle="toggleContent"
          :selectType="selectType" :onlyContent="onlyContent" :ratings="ratings"></ratingselect>
      <div class="rating-wrapper">
        <ul>
          <li v-for="rating in ratings" v-show="needShow(rating.rateType,rating.text)"
            class="rating-item">
            <div class="avatar">
              <img src="28" height="28" :src="rating.avatar">
            </div>
            <div class="content">
              <h1 class="name">{{rating.username}}</h1>
              <div class="star-wrapper">
                <star :size="24" :score="rating.score"></star>
                <span class="delivery" v-show="rating.deliveryTime">
                  {{rating.deliveryTime}}
                </span>
              </div>
              <p class="text">{{rating.text}}</p>
              <div class="recommend" v-show="rating.recommend && rating.recommend.length">
                <span class="icon-thumb_up"></span>
                <span class="item" v-for="item in rating.recommend">{{item}}</span>
              </div>
              <div class="time">
                {{rating.rateTime | formatDate}}
              </div>
            </div>
          </li>
        </ul>
      </div>
    </div>
  </div>
</template>

<script type="text/ecmascript-6">

  //1. 导入所需内容
  import BScroll from 'better-scroll';
  import { formatDate } from '../../common/js/date'
  import star from '../../components/star/star'
  import ratingselect from '../../components/ratingselect/ratingselect'
  import split from '../../components/split/split'

  //2. 定义常量
  const ALL = 2;
  const ERR_OK = 0;
  const debug = process.env.NODE_ENV !== 'production';

  export default{
    // 创建props，方便父组件传递数据
    props:{
      // 通过这种方式拿到 App.vue 中传递过来的 seller <router-view v-bind:seller="seller"></router-view>
      seller:{
        type:Object
      }
    },
    data(){
      return {
        ratings:[],
        selectType:ALL,
        onlyContent:true
      }
    },
    created(){
      const url = debug ? '/api/ratings' : 'http://ustbhuangyi.com/sell/api/ratings';
      this.$http.get(url).then((response) => {
        response = response.body;
        if(response.errno === ERR_OK){
          this.ratings = response.data;
          this.$nextTick(()=>{
            this.scroll = new BScroll(this.$refs.ratings,{
              click:true
            });
          });
        }
      });
    },
    methods:{
      needShow(type,text){
        if(this.onlyContent && !text){
          return false;
        }
        if(this.selectType === ALL){
          return true;
        }else{
          return type === this.selectType;
        }
      },
      selectRating(type){
        this.selectType = type;
        this.$nextTick(()=>{
          this.scroll.refresh();
        });
      },
      toggleContent(){
        this.onlyContent = !this.onlyContent;
        this.$nextTick(()=>{
          this.scroll.refresh();
        });
      }
    },
    filters:{
      formatDate(time){
        let date = new Date(time);
        return formatDate(date,'yyyy-MM-dd hh:mm');
      }
    },
    //组件注册，不注册使用会报错
    components:{
      star,split,ratingselect
    }
  };

</script>

<style>

</style>
