<template>
    <div class="ratingselect">
      <div class="rating-type border-1px">
        <!--全部-->
        <span @click="select(2,$event)" class="block positive" :class="{'active':selectType===2}">
          {{desc.all}}
          <span class="count">{{ratings.length}}</span>
        </span>
        <!--满意-->
        <span @click="select(2,$event)" class="block positive" :class="{'active':selectType===2}">
          {{desc.positive}}
          <span class="count">{{positives.length}}</span>
        </span>
        <!--不满意-->
        <span @click="select(2,$event)" class="block positive" :class="{'active':selectType===2}">
          {{desc.negative}}
          <span class="count">{{negatives.length}}</span>
        </span>
      </div>
    </div>
</template>

<script type="text/ecmascript-6">
  const POSITIVE = 0;
  const NEGATIVE = 1;
  const ALL = 2;

  export default {

    props:{
      ratings:{
        type:Array,
        default(){
          return [];
        }
      },
      selectType:{
        type:Number,
        default:ALL
      },
      onlyContent:{
        type:Boolean,
        default:false
      },
      desc:{
        type:Object,
        default(){
          return{
            all:'全部',
            positive:'满意',
            negative:'不满意'
          };
        }
      }
    },

    computed:{
      positives(){
        return this.ratings.filter((rating) => {
          return rating.rateType === POSITIVE;
        });
      },
      negatives(){
        return this.ratings.filter((ratings) => {
          return ratings.rateType === NEGATIVE;
        });
      }
    },

    methods:{
      select(type,event){
        if(!event._constructed){
          return;
        }
        this.$emit('select',type);
      },
      toggleContent(event){
        if(!event._constructed){
          return;
        }
        this.$emit('toggle');
      }
    }
  };
</script>

<style></style>

