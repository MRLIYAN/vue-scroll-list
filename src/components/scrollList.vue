<template>
    <div class="list-scroll-ctain">
        <div class="listScrollContentCtain" ref="listScrollContentCtain">
            <div class="listScrollContent" ref="listScrollContent">
                <slot>
                </slot>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    data() {
        return {
            i:0,
        }
    },
    props:{
        scrollOption:{
            
        }
    },
    mounted() {
        this.scroll();
    },
    methods:{
        scroll(){
            this.$nextTick(() => {
                let contH = this.$refs.listScrollContent.getBoundingClientRect().height.toFixed(2);
                let viewH = this.$refs.listScrollContentCtain.getBoundingClientRect().height.toFixed(2);
                let cha = contH - viewH;
                if(this.i < cha){
                    this.i = this.i + this.scrollOption.speed * 2;
                }else{
                    this.i = 0;
                }
                this.$refs.listScrollContent.style.transform = `translateY(${-this.i}px)`;
                window.requestAnimationFrame(this.scroll)
            })
        }
    }
}
</script>

<style lang="scss" scoped>
.list-scroll-ctain{
    width: 100%; height: 100%;
    &,& *{
        margin:0; padding: 0; box-sizing: border-box;
    }
    .listScrollContentCtain{
        width: 100%;
        height: 100%;
    }
}
</style>