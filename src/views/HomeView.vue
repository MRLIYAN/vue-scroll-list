<template>
    <div class="homeview">
        <div class="list-ctain">
            <scrollList :scrollOption="scrollOption">
                <ul>
                    <li v-for="count in 20" :key="count">{{ count }}、消息消息消息</li>
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
                speed:0.4,
            }
        }
    },
}
</script>

<style lang="scss" scoped>
.homeview {
    .list-ctain {
        width: 400px;
        height: 400px;
        border: 1px solid red;
        overflow: hidden;
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