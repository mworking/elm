<template>
    <div class="star" v-bind:class="starType">
        <span v-for="itemClass in itemClasses" class="star-item" v-bind:class="itemClass" track-by="$index">
        </span>
    </div>
</template>

<script type="text/ecmascript-6">

    const LENGTH = 5;
    const CLS_ON = 'on';
    const CLS_HALF = "half";
    const CLS_OFF = "off";

    export default {
      // 通过父组件  <star :size="36" :score="seller.serviceScore"></star> 向子组件传值， 可以这么想，子组件 props 中申明了这些内容，父组件调用传值
        props:{
            size:{
                type:Number
            },
            score:{
                type:Number
            }
        },
        computed:{
            starType(){
                return 'star-' + this.size;
            },
            itemClasses(){
                let result = [];
                let score = Math.floor(this.score * 2) /2 ;
                let hasDecimal = score % 1 !== 0; //是否有小数
                let integer = Math.floor(score);
                for(let i =0; i<integer; i++){
                    result.push(CLS_ON);
                }
                if(hasDecimal){
                    result.push(CLS_HALF);
                }
                while (result.length < LENGTH){
                    result.push(CLS_OFF);
                }
                console.log(result)
                return result;
            }
        }
    }

</script>

<style lang="scss" scoped>
    .star{
        font-size: 0;
        .star-item{
            display: inline-block;
            background-repeat: no-repeat;
        }
    }
    .star-48{
        .star-item{
            width: 20px;
            height: 20px;
            margin-right: 22px;
            background-size: 20px 20px;
        }
        .on{
            background-image: url("star48_on@2x.png");
        }
        .half{
            background-image: url("star48_half@2x.png");
        }
        .off{
            background-image: url("star48_off@2x.png");
        }
    },
    .star-48:last-child{
        margin-right: 0;
    }
    .star-36{
        .star-item{
            width: 20px;
            height: 20px;
            margin-right: 6px;
            background-size: 15px 15px;
        }
        .on{
            background-image: url("star36_on@2x.png");
        }
        .half{
            background-image: url("star36_half@2x.png");
        }
        .off{
            background-image: url("star36_off@2x.png");
        }
    },
    .star-36:last-child{
        margin-right: 0;
    }
    .star-24{
        .star-item{
            width: 10px;
            height: 10px;
            margin-right: 3px;
            background-size: 10px 10px;
        }
        .on{
            background-image: url("star24_on@2x.png");
        }
        .half{
            background-image: url("star24_half@2x.png");
        }
        .off{
            background-image: url("star24_off@2x.png");
        }
    }
    .star-24:last-child{
        margin-right: 0;
    }
</style>

