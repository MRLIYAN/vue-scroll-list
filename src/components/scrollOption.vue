<template>
    <div class="optionContainer">
        <div class="option-ctain">
            <h2>option配置项：</h2>
            <table>
                <thead>
                    <tr>
                        <th>配置项</th>
                        <th>默认值</th>
                        <th>值</th>
                        <th>描述</th>
                    </tr>
                </thead>

                <tbody>
                    <tr v-for="(item, index) in option" :key="index">
                        <td>{{item.key}}:</td>
                        <td>{{item.defaultVal}}</td>
                        <td>{{item.val}}</td>
                        <td>{{item.msg}}</td>
                    </tr>
                </tbody>
            </table>
        </div>

        <div class="option-ctain">
            <h2>函数配置项：</h2>
            <table>
                <thead>
                    <tr>
                        <th>配置项</th>
                        <th>默认值</th>
                        <th>值</th>
                        <th>描述</th>
                    </tr>
                </thead>

                <tbody>
                    <tr v-for="(item, index) in funcOption" :key="index">
                        <td>{{item.key}}:</td>
                        <td>{{item.defaultVal}}</td>
                        <td>{{item.val}}</td>
                        <td>{{item.msg}}</td>
                    </tr>
                </tbody>
            </table>
        </div>
        
    </div>
</template>

<script>
export default {
    data() {
        return {
            option:[
                {
                    key:'speed',
                    defaultVal:'5',
                    val:'0~100',
                    msg:'滚动速度 0~100 数字越大速度越快，默认5'
                },
                {
                    key:'hoverStop',
                    defaultVal:'false',
                    val:'true / false',
                    msg:'是否开启鼠悬浮、滚动停止，默认关闭'
                },
                {
                    key:'loop',
                    defaultVal:'true',
                    val:'true / false',
                    msg:'是否无缝滚动，默认开启'
                },
                {
                    key:'direction',
                    defaultVal:'up',
                    val:'up、down、left、right',
                    msg:'滚动方向，up向上 down向下 left向左 right向右；默认向上'
                },
            ],
            funcOption:[
                {
                    key:'@scrollEnd',
                    defaultVal:'function',
                    val:'function',
                    msg:'回调函数，当向上滚动，设置为非无缝滚动时，滚动到底部会触发回调，通知已滚动到底部'
                }
            ]
        }
    },
}
</script>

<style lang="scss" scoped>
.optionContainer{

    .option-ctain{
        width:1200px;
        padding:20px;
        margin:0 auto;

        h2{
            padding:20px 0;
            text-align: left;
        }
    }

    table{
        width: 100%;
        border-collapse: collapse;

        thead{
            background: rgba(220,220,220,1);
        }
        th,td{
            border:1px solid rgba(236, 236, 236, 1);
            line-height: 40px;
            padding:5px 10px;
            text-align: left;
        }
    }

}
</style>