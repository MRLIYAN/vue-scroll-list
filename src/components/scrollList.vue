<template>
    <div class="list-scroll-ctain">
        <div class="listScrollContentCtain" ref="listScrollContentCtain">
            <div class="listScrollContent" ref="listScrollContent">
                <div>
                    <slot></slot>
                </div>
                <div v-if="isContH && option.loop">
                    <slot></slot>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    name:'scrollList',
    data() {
        return {
            i:0,
            isContH:false,
            speed:1,
            isScroll:true,
            option:{}
        }
    },
    props:['scrollOption'],
    computed: {
       
    },
    mounted() {
        //默认设置无缝滚动
        this.$set(this.option, 'loop', true);
        this.option = {...this.scrollOption}
        console.log(this.option);

        //限制速度，设置1~10
        if(this.option.speed < 1) { 
            this.option.speed = 1;
        }
        if(this.option.speed > 10){
            this.option.speed = 10;
        }

        this.$nextTick(() => {
            this.contView().style.height = this.slot0().clientHeight+'px';
            //判断初始化是否需要滚动，以及展示第二个
            if(this.slot0().clientHeight < this.viewH){
                this.isScroll =false;
                this.isContH = false;
            }else{
                this.scroll();
                this.isContH = true;
            }

            if(this.option.hoverStop){
                this.contView().onmouseenter = () => {
                    this.isScroll = false;
                }
                this.contView().onmouseleave = () => {
                    this.isScroll = true;
                    this.scroll();
                }
            }
        })
        
    },
    methods:{
        contH(){ //内容高度
            return this.$refs.listScrollContent.getBoundingClientRect().height;
        },
        viewH(){ //视图高度
            return this.$refs.listScrollContentCtain.getBoundingClientRect().height;
        },
        contView(){ //内容dom
            return this.$refs.listScrollContent;
        },
        slot0(){ //插槽1
            return this.$refs.listScrollContent.children[0];
        },
        slot1(){ //插槽2
            return this.$refs.listScrollContent.children[1];
        },
        scroll(){
            this.$nextTick(() => {
                if(this.isScroll){
                    let contH = this.contH();
                    let viewH = this.viewH();
                    let cha = 0;
                    
                    if(this.option.loop){
                        cha = contH;
                        if(this.i < cha){
                            this.i = this.i + this.option.speed * 0.4;
                        }else{
                            this.i = 0;
                        }
                    }else{
                        cha = contH - viewH;
                        if(this.i < cha){
                            this.i = this.i + this.option.speed * 0.4;
                        }else{
                            this.i = 0;
                        }
                    }
                    
                    this.contView().style.transform = `translateY(${-this.i}px)`;
                    window.requestAnimationFrame(this.scroll)
                }
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