<template>
    <div class="listScrollContainer" ref="listScrollContainer">
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
            isScroll:true,
            option:{}
        }
    },
    props:{
        scrollOption: {
            type:Object,
            default:() => {
                return {}
            }
        }
    },
    computed:{
        view(){ // 最外层视图
            return this.$refs.listScrollContainer;
        },
        viewH(){ //最外层视图高度
            return this.$refs.listScrollContainer.clientHeight;
        },
        contView(){ //内容视图
            return this.$refs.listScrollContentCtain;
        },
        cont(){ //内容
            return this.$refs.listScrollContent;
        },
        contH(){ //内容高度
            return this.$refs.listScrollContent.clientHeight; 
        },
        slot0(){ //插槽1
            return this.$refs.listScrollContent.children[0];
        },
        slot1(){ //插槽2
            return this.$refs.listScrollContent.children[1];
        },
    },
    mounted() {
        //默认设置无缝滚动
        this.$set(this.option, 'loop', true);
        this.option = {...this.scrollOption}
        console.log(this.option);

        //限制速度，设置0~100
        if(this.option.speed <= 0) { 
            this.option.speed = 0;
        }
        if(this.option.speed >= 100){
            this.option.speed = 100;
        }


        this.$nextTick(() => {
            if(this.option.loop){
                // 给视图添加高度，高度是内容的插槽的一个高度，防止无缝滚动外边添加滚动条会导致高度撑开超出，滚动底部出现留白。
                this.contView.style.height = this.slot0.clientHeight+'px';
            }
           
            //判断初始化是否需要滚动，以及展示第二个
            if(this.slot0.clientHeight < this.viewH){
                this.isScroll =false;
                this.isContH = false;
            }else{
                this.scroll();
                this.isContH = true;
            }

            if(this.option.hoverStop){
                this.view.onmouseenter = () => {
                    this.isScroll = false;
                }
                this.view.onmouseleave = () => {
                    this.isScroll = true;
                    this.scroll();
                }
            }
        })
        
    },
    methods:{
        scroll(){
            this.$nextTick(() => {
                if(this.isScroll){
                    let contH = this.contH;
                    let viewH = this.viewH;
                    let cha = 0;
                    
                    if(this.option.loop){
                        //无缝滚动，获取内容高度
                        cha = this.slot0.clientHeight;
                        if(this.i < cha){
                            this.i = this.i + this.option.speed * 0.2;
                        }else{
                            this.i = 0;
                            // this.$emit('scrollEnd','aa');
                        }
                    }else{
                        cha = contH - viewH;
                        if(this.i < cha){
                            this.i = this.i + this.option.speed * 0.2;
                        }else{
                            this.i = 0;
                            // 非无缝滚动模式下，滚动到底部触发scrollEnd回调，可以监听是否滚动到底部
                            this.$emit('scrollEnd','aa');
                        }
                    }
                    
                    this.cont.style.transform = `translateY(${-this.i}px)`;
                    window.requestAnimationFrame(this.scroll)
                }
            })
        }
    }
}
</script>

<style lang="scss" scoped>
.listScrollContainer{
    width: 100%; height: 100%;
    &,& *{
        margin:0; padding: 0; box-sizing: border-box;
    }
    .listScrollContentCtain{
        width: 100%;
        overflow: hidden;
    }
}
</style>