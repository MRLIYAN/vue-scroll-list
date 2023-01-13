<template>
    <div class="homeview">
        <div class="view">
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
            <button @click="pause1">暂停动画</button>
        </div>
        

        <div class="view">
            <div class="list-ctain">
                <scrollList 
                    :scrollOption="scrollOption2"
                    @scrollEnd="scrollEnd"
                >
                    <ul>
                        <li v-for="count in 15" :key="count" @click="msg(count)">{{ count }}、消息消息消息</li>
                    </ul>
                </scrollList>
            </div>
            <button @click="pause2">暂停动画</button>
        </div>
        


        <div class="view">
            <div class="list-ctain-h">
                <scrollList 
                    :scrollOption="scrollOption3"
                    @scrollEnd="scrollEnd"
                >
                    <div class="list-news">
                        <span v-for="count in 5" :key="count">{{count}}、这是第{{count}}条消息</span>
                    </div>
                </scrollList>
            </div>
        </div>

        <div class="view">
            <div class="list-ctain-h">
                <scrollList 
                    :scrollOption="scrollOption4"
                    @scrollEnd="scrollEnd"
                >
                    <div class="list-news">
                        <span v-for="count in 5" :key="count">{{count}}、这是第{{count}}条消息</span>
                    </div>
                </scrollList>
            </div>
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
                speed:4,
                hoverStop:true,
                loop:true, 
                direction:'up',
                pause:false,
            },
            scrollOption2:{
                speed:4,
                hoverStop:true,
                loop:true, 
                direction:'down',
                pause:false,
            },
            scrollOption3:{
                speed:7,
                hoverStop:true,
                loop:true, 
                direction:'left',
            },
            scrollOption4:{
                speed:7,
                hoverStop:true,
                loop:false, 
                direction:'right',
            }
        }
    },
    methods:{
        msg(val){
            alert(val)
        },
        pause1(){
            this.scrollOption.pause = !this.scrollOption.pause
        },
        pause2(){
            this.scrollOption2.pause = !this.scrollOption2.pause
        },
        scrollEnd(){
            console.log('滚动到底部了');
        }
    }
}
</script>

<style lang="scss" scoped>
.homeview {
    .view{
        display: inline-block;
        margin:40px;
    }
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
        margin-top: 40px;
        overflow:hidden;
        .list-news{
            
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