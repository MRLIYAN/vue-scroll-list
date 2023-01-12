<template>
    <div class="listScrollContainer" ref="listScrollContainer">
        <!-- <div class="listScrollContentCtain" ref="listScrollContentCtain"> -->
            <div :class="['listScrollContent','listScrollContent'+option.direction]" ref="listScrollContent">
                <div>
                    <slot></slot>
                </div>
                <div v-if="isContH && option.loop">
                    <slot></slot>
                </div>
            </div>
        <!-- </div> -->
    </div>
</template>

<script>

export default {
    name: 'scrollList',
    data() {
        return {
            i: 0,
            isContH: false,
            isScroll: true, //是否允许滚动
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
            return this.$refs.listScrollContainer.clientHeight;
        },
        viewW() { //最外层视图宽度
            return this.$refs.listScrollContainer.clientWidth;
        },
        cont() { //内容
            return this.$refs.listScrollContent;
        },
        contH() { //内容高度
            return this.$refs.listScrollContent.clientHeight;
        },
        contW() { //内容高度
            return this.$refs.listScrollContent.clientWidth;
        },
        slot0() { //插槽1
            return this.$refs.listScrollContent.children[0];
        },
        slot0H() { //插槽1高度
            return this.$refs.listScrollContent.children[0].clientHeight;
        },
        slot0W() { //插槽1高度
            return this.$refs.listScrollContent.children[0].clientWidth;
        },
        slot1() { //插槽2
            return this.$refs.listScrollContent.children[1];
        },
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
                direction: this.scrollOption.direction ? this.scrollOption.direction : 'up'
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
                if (this.option.loop && (this.option.direction == 'up' || this.option.direction == 'down') ) {
                    // 给视图添加高度，高度是内容的插槽的一个高度，防止无缝滚动外边添加滚动条会导致高度撑开超出，滚动底部出现留白。
                    this.view.style.height = this.slot0.clientHeight + 'px';
                }

                //判断初始化是否需要滚动，以及展示第二个
                if(this.option.direction == 'up') {
                    if (this.slot0H < this.viewH) { //视图是否超出
                        this.isScroll = false;
                        this.isContH = false;
                    } else {
                        this.isContH = true;
                        this.scroll();
                    }
                }else if(this.option.direction == 'down') {
                    this.i = this.slot0H;
                    if (this.slot0H < this.viewH) { //视图是否超出
                        this.isScroll = false;
                        this.isContH = false;
                    } else {
                        this.isContH = true;
                        this.scroll();
                    }
                }

                if (this.option.hoverStop) {
                    this.view.onmouseenter = () => {
                        this.isScroll = false;
                    }
                    this.view.onmouseleave = () => {
                        if (this.isContH) { //内容超出视图才允许滚动
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
                cha = this.slot0.clientHeight;
                if (this.i < cha) {
                    this.i = this.i + this.option.speed * 0.2;
                } else {
                    this.i = 0;
                    // this.$emit('scrollEnd','aa');
                }
            } else {
                cha = contH - viewH;
                if (this.i < cha) {
                    this.i = this.i + this.option.speed * 0.2;
                } else {
                    this.i = 0;
                    // 非无缝滚动模式下，滚动到底部触发scrollEnd回调，可以监听是否滚动到底部
                    this.$emit('scrollEnd', 'aa');
                }
            }

            this.cont.style.transform = `translateY(${-this.i}px)`;
            window.requestAnimationFrame(this.scroll)
        },
        scrollDown(){
            let contH = this.slot0H;
            if (this.option.loop) {
                //无缝滚动，获取内容高度
                if (this.i > 0) {
                    this.i = this.i - this.option.speed * 0.2;
                } else {
                    this.i = contH;
                }
            }

            this.cont.style.transform = `translateY(${-this.i}px)`;
            window.requestAnimationFrame(this.scroll)
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
    // width: 100%;
    // height: 100%;
    overflow: hidden;

    &,
    & * {
        margin: 0;
        padding: 0;
        box-sizing: border-box;
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