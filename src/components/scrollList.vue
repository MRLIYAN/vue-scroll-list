<template>
    <!-- 最外层视图，默认宽高100%，方便获取使用者设置的视图宽高，判断内容是否超出视图 -->
    <div class="listScrollContainer" ref="listScrollContainer">
        <!-- 内容视图，设置内容的宽度高度，防止克隆内容导致的宽高会默认两倍 -->
        <div class="listScrollContentContain" ref="listScrollContentContain">
            <!-- 真实内容宽高，无论克隆不可隆内容，或者插槽2是否展示，内容占据的实际宽高，并且是移动的对象 -->
            <div :class="['listScrollContent','listScrollContent'+option.direction]" ref="listScrollContent">
                <div>
                    <slot></slot>
                </div>
                <div v-if="isShow">
                    <slot></slot>
                </div>
            </div>
        </div>
    </div>
</template>

<script>

export default {
    name: 'scrollList',
    data() {
        return {
            i: 0,
            isShow: false, // 是否展示克隆内容
            isScroll: true, // 是否允许滚动
            isOverView:false, // 内容是否超出视图
            option: {}
        }
    },
    props: {
        scrollOption: {
            type: Object,
            default: () => {
                return {}
            }
        }
    },
    computed: {
        view() { // 最外层视图
            return this.$refs.listScrollContainer;
        },
        viewH() { //最外层视图高度
            return this.$refs.listScrollContainer.getBoundingClientRect().height;
        },
        viewW() { //最外层视图宽度
            return this.$refs.listScrollContainer.getBoundingClientRect().width;
        },
        contCtain(){ //内容视图
            return this.$refs.listScrollContentContain;
        },
        cont() { //内容
            return this.$refs.listScrollContent;
        },
        contH() { //内容高度
            return this.$refs.listScrollContent.getBoundingClientRect().height;
        },
        contW() { //内容高度
            return this.$refs.listScrollContent.getBoundingClientRect().width;
        },
        slot0() { //插槽1
            return this.$refs.listScrollContent.children[0];
        },
        slot0H() { //插槽1高度
            return this.$refs.listScrollContent.children[0].getBoundingClientRect().height;
        },
        slot0W() { //插槽1高度
            return this.$refs.listScrollContent.children[0].getBoundingClientRect().width;
        },
        slot1() { //插槽2
            return this.$refs.listScrollContent.children[1];
        },
    },
    watch:{
        'scrollOption.pause'(){
            this.initProps();
        },
        'option.pause'(){
            if(this.option.pause){
                this.isScroll = false;
            }else{
                this.isScroll = true;
                if(this.isOverView){//所有滚动的前提必须是内容必须超出视图才可以滚动
                    this.scroll();
                }
            }
        }
    },
    mounted() {
        //默认参数
        this.initProps();
        this.initScroll();
    },
    methods: {
        initProps() {
            this.option = {
                //滚动速度 0~100 数字越大速度越快，默认5
                speed: this.scrollOption.speed ? this.scrollOption.speed : 5,
                //是否开启鼠标悬停,默认关闭
                hoverStop: this.scrollOption.hoverStop ? ((this.scrollOption.hoverStop === 'true' || this.scrollOption.hoverStop === true) ? true : false) : false,
                //是否无缝滚动，默认开启
                loop: (this.scrollOption.loop) ? ((this.scrollOption.loop === 'true' || this.scrollOption.loop === true) ? true : false) : ((this.scrollOption.loop === 'false' || this.scrollOption.loop === false) ? false : true),
                //滚动方向,默认向上 up向上 down向下 left向左 right向右
                direction: this.scrollOption.direction ? this.scrollOption.direction : 'up',
                //设置动画是否暂停，默认不暂停即false (true暂停动画，false关闭动画)
                pause:this.scrollOption.pause ? ((this.scrollOption.pause === 'true' || this.scrollOption.pause === true) ? true : false) : false,
            }
            //限制速度，设置0~100
            if (this.option.speed <= 0) {
                this.option.speed = 0;
            }
            if (this.option.speed >= 100) {
                this.option.speed = 100;
            }
            console.log(this.option);
        },
        initScroll() {
            this.$nextTick(() => {
                //内容是否超出视图
                this.isOverView = this.slot0H > this.viewH ? true : false;

                /**
                 * 开启无缝滚动，向上，向下的时候，给视图设置高度，高度是内容的插槽的一个高度，
                 * 防止无缝滚动克隆内容导致的高度是内容高度的两倍
                 * 防止无缝滚动外边添加滚动条会导致高度撑开超出，滚动底部出现留白。
                 */
                if (this.option.loop && (this.option.direction == 'up' || this.option.direction == 'down') ) {
                    this.contCtain.style.height = this.slot0H + 'px';
                }

                //判断初始化是否需要滚动，以及展示第二个
                if(this.option.direction == 'up') {
                    //是否展示克隆内容，或者插槽2内容
                    this.isShow = this.option.loop ? true : false;
                    //根据视图超出判断默认是否滚动，所有滚动的前提必须是内容必须超出视图才可以滚动
                    this.isScroll = this.isOverView ? true : false;

                    if(this.isScroll){
                        this.scroll();
                    }

                }else if(this.option.direction == 'down') {
                    //是否展示克隆内容，或者插槽2内容
                    this.isShow = this.option.loop ? true : false;
                    //根据视图超出判断默认是否滚动
                    this.isScroll = this.isOverView ? true : false;

                    if(!this.option.loop){
                        this.i = this.slot0H - this.viewH;
                    }

                    if(this.isScroll){
                        this.scroll();
                    }
                }

                //设置鼠标hover悬停
                if (this.option.hoverStop) {
                    this.view.onmouseenter = () => {
                        this.isScroll = false;
                    }
                    this.view.onmouseleave = () => {
                        //滚动的首要前提是必须内容超过视图高度
                        if(this.isOverView){
                            this.isScroll = true;
                            this.scroll();
                        }
                       
                    }
                }
            })
        },
        scroll() {
            this.$nextTick(() => {
                if (this.isScroll) {
                    if(this.option.direction == 'up'){
                        this.scrollUp();
                    }else if(this.option.direction == 'down'){
                        this.scrollDown();
                    }else if(this.option.direction == 'left'){
                        this.scrollLeft();
                    }else if(this.option.direction == 'right'){
                        this.scrollRight();
                    }
                }
            })
        },
        scrollUp() {
            let contH = this.contH; //内容高度
            let viewH = this.viewH; //视图高度
            let cha = 0;

            if (this.option.loop) {
                //无缝滚动，获取内容高度
                cha = this.slot0H;
                if (this.i < cha) {
                    this.i = this.i + this.option.speed * 0.2;
                } else {
                    this.i = 0;
                    // this.$emit('scrollEnd');
                }
            } else {
                //非无缝滚动，获取内容高度和视图高度的计算偏差，作为移动偏移量判断
                cha = contH - viewH;
                if (this.i < cha) {
                    this.i = this.i + this.option.speed * 0.2;
                } else {
                    this.i = 0;
                    // 非无缝滚动模式下，滚动到底部触发scrollEnd回调，可以监听是否滚动到底部
                    this.$emit('scrollEnd');
                }
            }

            this.cont.style.transform = `translateY(${-this.i}px)`;
            if(this.isScroll){
                window.requestAnimationFrame(this.scroll);
            }
        },
        scrollDown(){
            if (this.option.loop) {
                //无缝滚动，获取内容高度
                let contH = this.slot0H;
                if (this.i > 0) {
                    this.i = this.i - this.option.speed * 0.2;
                } else {
                    this.i = contH;
                }
            }else{
                let cha = this.slot0H - this.viewH;
                if(this.i > 0){
                    this.i = this.i - this.option.speed * 0.2;
                } else {
                    this.i = cha
                }
            }

            this.cont.style.transform = `translateY(${-this.i}px)`;
            if(this.isScroll){
                window.requestAnimationFrame(this.scroll)
            }
        },
        scrollLeft(){

        },
        scrollRight(){

        }
    }
}
</script>

<style lang="scss" scoped>
.listScrollContainer {
    width: 100%;
    height: 100%;

    &,
    & * {
        margin: 0;
        padding: 0;
        box-sizing: border-box;
    }

    .listScrollContentContain{
        overflow: hidden;
    }

    .listScrollContentleft,.listScrollContentright{
        display: flex;
        justify-content: flex-start;
        align-items: center;
        flex-wrap: nowrap;
        div{
            flex-shrink: 0;
        }
    }
}
</style>