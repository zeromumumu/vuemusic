<template>
    <div class="listView">
        <!-- 歌单详情 -->
        <listview-top :playlist="music.playlist"></listview-top>
        <!-- 歌曲列表 -->
        <play-list :playlist="music.playlist"></play-list>
    </div>
</template>
<script>
import { useRoute } from 'vue-router'
import { onMounted ,reactive} from 'vue';
import { getBanner3 } from "@/api/index.js" //@指的是项目目录  src
import listviewTop from "@/components/ListViewTop.vue"
import playList from "@/components/PlayList.vue"
export default{
    name:"listview",
    setup(){
        var route = useRoute(); //当前路由信息对象 等价于 $route
        var music = reactive({
            list:[],
            playlist:{
                creator:{ },//歌单详情
                tracks:[ ] //播放列表
            }
        })
        onMounted(async() => {
            const id = route.query.id; //当前歌单的id
            console.log(id);
            var res = await getBanner3(id); //发生axios请求，获取歌单详情
            music.playlist = res.data.playlist;
            console.log(music.playlist);
        })
        return {music}
    },
    components:{
        listviewTop,
        playList
    }

}
</script>