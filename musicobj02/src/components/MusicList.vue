<template>
    <!-- <div id="list">
        <div id="swiper-container1">
            <div class="swiper-wrapper">
            <div class="w1 swiper-slide" style="float: left;margin-left: 20px;" v-for="(item, index) in imgs" :key="index">
                <img style="width: 100px;height: 100px; float: left;" :src="item.picUrl" alt="">
                <p style="width: 100px;">{{ item.name }}</p>
            </div>
        </div>
        </div>
    </div> -->
    <div class="musicList">
        <div class="musicList-top">
            <div class="title">发现好歌单</div>
            <div class="more">查看更多</div>
        </div>
        <div class="mlist">
            <div class="swiper-container" id="musicSwiper">
                <div class="swiper-wrapper">
                    <div class="swiper-slide" v-for="item in musicls.musics" :key="item.id">
                        <img :src="item.picUrl" alt="">
                        <div class="name">{{ item.name }}</div>
                        <div class="count">
                            <svg class="icon" aria-hidden="true">
                                <use xlink:href="#icon-fanjutuijian"></use>
                            </svg>
                            <span>{{changeValue(item.playCount) }}</span>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
import "swiper/css/swiper.css"
import Swiper from "swiper"
import { getBanner2 } from "@/api/index.js"
import { onMounted, onUpdated, reactive} from "vue"
export default {
    name: 'musicList',
    // data(){
    //     return {
    //         imgs:[
    //         { pic: require("../assets/logo.png") },
    //         { pic: require("../assets/logo.png") },
    //         { pic: require("../assets/logo.png") },
    //         { pic: require("../assets/logo.png") },
    //         { pic: require("../assets/logo.png") },
    //         { pic: require("../assets/logo.png") },

    //         ],
    //         name:"11",

    //     }

    // },
    setup(){
        const musicls = reactive({musics:[] });
        function changeValue(num){  //格式化播放量  55555555555
            var res = 0;
            if(num>=100000000){
                res = num/100000000;
                res = res.toFixed(2) + "亿";  //toFixed(2)截取小数点后2位
            }else if(num >=10000){
                res = num/10000;
                res = res.toFixed(2) + "万";
            }else{
                res = num;
            }
            return res;
        }
        onMounted(async() => {   //view与model绑定成功之后
            var res = await getBanner2(6);
            musicls.musics = res.data.result;
            console.log(musicls.musics);
        }),
        onUpdated(() => {
            var swiper = new Swiper("#musicSwiper",{
                slidesPerView:3,    //一个屏显示几个滑块
                spaceBetween:10     //每个滑块之间的间距
                
            })
        })
        return {musicls,changeValue}
    },

}
    // data() {
    //     return {
    //         imgs: [
    //             { pic: require("../assets/logo.png") },
    //             { pic: require("../assets/logo.png") },
    //             { pic: require("../assets/logo.png") },
    //             { pic: require("../assets/logo.png") },
    //             { pic: require("../assets/logo.png") },

    //         ]
    //     }
    // },
//     async mounted() {
//         var res = await getBanner2(6);
//         this.imgs = res.data.result;
//         var myswiper = new Swiper('#swiper-container1', {
//             observer: true,
//        observeSlideChildren: true,
//        observeParents: true, //这三个可以去文档看下介绍，主要就是异步情况会重新加载swiper。
//       loop: true, //开启环路
//       spaceBetween: 30, //每个轮播图间隔
//       slidesPerView: "auto", //当前页面显示几个，这里是auto
//       loopedSlides: 5, //比你页面显示的数量大于2就好
//       updateOnWindowResize: false, //分辨率改变时，防止样式错乱加上！
//         })
//     }
// }
</script>

<!-- <style>
#swiper-container1 {
    display: flex;
    overflow: hidden;
    position: relative;
}

.w1 {
    width: 30%;
    position: relative;
}
</style>  -->

<style lang="less" scoped>
.musicList{
    width: 7.5rem;
    padding: 0 0.4rem;
    .musicList-top{
        display: flex;
        justify-content: space-between;
        height: 1rem;
        align-items: center;
        .title{
            font-size: 0.4rem;
            font-weight: 900;
        }
        .more{
            border: 1px solid #ccc;
            border-radius: 0.2rem;
            font-size: 0.24rem;
            height: 0.5rem;
            width: 1.2rem;
            text-align: center;
            line-height: 0.5rem;
        }
    }
    .mlist{
        .swiper-container{
            width: 100%;
            height: 3rem;
            .swiper-slide{
                
                display: flex;
                flex-direction: column;
                position: relative;
                img{
                    width:100%;
                    height: auto;
                    border-radius: 0.1rem;
                }
                .name{
                    height: 0.6rem;
                    width: 100%;
                    font-size: 0.24rem;
                    line-height: 0.4rem;
                }
                .count{
                    position:absolute;
                    right: 0.1rem;
                    top: 0.1rem;
                    font-size: 0.24rem;
                    color: #ccc;
                    display: flex;
                    align-items: center;
                    .icon{
                         fill: #ccc;
                    }
                }
            }
        }
        
    }
}
</style>
