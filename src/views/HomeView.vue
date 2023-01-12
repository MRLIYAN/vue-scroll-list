<template>
    <div class="homeview">
        <div class="list-ctain">
            <scrollList 
                :scrollOption="scrollOption"
                @scrollEnd="scrollEnd"
            >
                <ul>
                    <li v-for="count in 15" :key="count" @click="msg(count)">{{ count }}、消息消息消息</li>
                </ul>
            </scrollList>
        </div>


        <div class="list-ctain-h">
            <scrollList 
                :scrollOption="scrollOption2"
                @scrollEnd="scrollEnd"
            >
                <div class="list-news">
                    <span v-for="count in 5" :key="count">{{count}}、这是第{{count}}条消息</span>
                </div>
            </scrollList>
        </div>
    </div>
</template>

<script>
import scrollList from '@/components/scrollList.vue';
export default {
    components: {
        scrollList
    },
    data() {
        return {
            scrollOption:{
                speed:5,//滚动速度 0~100 数字越大速度越快
                hoverStop:true,//是否开启鼠标悬停
                loop:true, //是否无缝滚动，默认开启
                direction:'up',//滚动方向
            },
            scrollOption2:{
                speed:5,//滚动速度 0~100 数字越大速度越快
                hoverStop:true,//是否开启鼠标悬停
                loop:true, //是否无缝滚动，默认开启
                direction:'left',//滚动方向
            }
        }
    },
    methods:{
        msg(val){
            alert(val)
        },
        scrollEnd(val){
            console.log(val);
        }
    }
}
</script>

<style lang="scss" scoped>
.homeview {
    .list-ctain {
        width: 400px;
        height: 400px;
        border: 1px solid red;
        overflow: auto;
    }
    .list-ctain::-webkit-scrollbar{
        width:5px;
        border-radius:5px;
    }
    .list-ctain::-webkit-scrollbar-track{
        width:5px;
        border-radius: 5px;
        background:rgba(255,255,255,.9);
    }
    .list-ctain::-webkit-scrollbar-thumb{
        width:5px;
        border-radius: 5px;
        background:rgba(0,0,0,.4);
    }

    ul {
        li {
            line-height: 30px;
            background-color: rgba(0, 0, 0, .1);
            margin: 2px 0;
        }
    }


    .list-ctain-h{
        width:900px;
        border:1px solid red;
        overflow-x: scroll;
        .list-news{
            margin-top: 40px;
            display: flex;
            justify-content: flex-start;
            align-items: center;
            flex-wrap: nowrap;
            

            span{
                display: inline-block;
                line-height: 40px;
                width:200px;
                flex-shrink: 0;
                
            }
        }
    }
    
}
</style>