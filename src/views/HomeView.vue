<template>
    <div class="homeview">
        <div class="list-ctain">
            <scrollList 
                :scrollOption="scrollOption"
                @scrollEnd="scrollEnd"
            >
                <ul>
                    <li v-for="count in 20" :key="count" @click="msg(count)">{{ count }}、消息消息消息</li>
                </ul>
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
                speed:1.5,//滚动速度 1~10 数字越大速度越快
                hoverStop:true,//是否开启鼠标悬停
                loop:false, //是否无缝滚动，默认开启
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

    ul {
        li {
            line-height: 30px;
            background-color: rgba(0, 0, 0, .1);
            margin: 2px 0;
        }
    }
}
</style>