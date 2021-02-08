<template>
    <div class="">
        <div class="first-floor">
            <form
                class="form"
                onsubmit="return false;"
            >
                <div class="form-item">
                    <label for="buyPrice">买入价格：</label>
                    <div>
                        <input
                            id="buyPrice"
                            type="number"
                            name="price"
                            v-model="buyPrice"
                        />
                        <span class="form-info">元/股</span>
                    </div>
                </div>
                <div class="form-item">
                    <label for="buyNum">买入数量：</label>
                    <div>
                        <input
                            id="buyNum"
                            type="number"
                            name="price"
                            v-model="buyNum"
                        />
                        <span class="form-info">手</span>
                    </div>

                </div>
                <div class="form-item">
                    <label for="sellPrice">卖出价格：</label>
                    <div>
                        <input
                            id="sellPrice"
                            type="number"
                            name="price"
                            v-model="sellPrice"
                        />
                        <span class="form-info">元/股</span>
                    </div>

                </div>
                <div class="form-item">
                    <label for="sellNum">卖出数量：</label>
                    <div>
                        <input
                            id="sellNum"
                            type="number"
                            name="price"
                            v-model="sellNum"
                        />
                        <span class="form-info">手</span>
                    </div>
                </div>
                <div class="form-item">
                    <label for="ratio">券商佣金比率：</label>
                    <div>
                        <input
                            id="ratio"
                            type="number"
                            name="price"
                            v-model="ratio"
                            step="0.1"
                        />
                        <span class="form-info">‱</span>
                    </div>

                </div>
                <div class="form-item">
                    <label for="stampDutyRate">印花税比率：</label>
                    <div>
                        <input
                            id="stampDutyRate"
                            type="number"
                            name="price"
                            v-model="stampDutyRate"
                            step="0.1"
                        />
                        <span class="form-info">%</span>
                    </div>

                </div>
                <div class="form-btn-box">
                    <button @click="reset">重置</button>
                </div>
            </form>
            <div>
                <h3>设置</h3>
                买入单位：
            </div>
        </div>
        <div>
            <p>计算结果</p>
            <ul>
                <li>券商佣金：<span class="bold ft16">{{brokerageCommission|number}}</span>元</li>
                <li>印花税：<span class="bold ft16">{{stampDuty|number}}</span>元</li>
                <li>税费合计：<span class="bold ft16">{{feesTotal|number}}</span>元</li>
                <li>投资收益：<span class="bold ft16">{{earnings|number}}</span>元</li>
                <li>盈亏比例：<span class="bold ft16">{{profitAndLossRatio|number}}</span>%</li>
            </ul>
        </div>
    </div>
</template>

<script>
// import  from ''
// import { mapState,mapGetters,mapActions,mapMutations } from 'vuex'

export default {
    // 组件名 每个文件的name都应该是唯一的
    name: "index",
    // 实例的数据对象
    data() {
        return {
            buyPrice: 0,
            buyNum: 1,
            sellPrice: 0,
            sellNum: 1,
            ratio: 2.5,
            stampDutyRate: 0.1, // 印花税率
        };
    },
    // 数组或对象，用于接收来自父组件的数据
    props: {},
    // 计算
    computed: {
        // 买入总金额
        buyTotal() {
            return this.buyPrice * this.buyNum * 100;
        },
        // 卖出总金额
        sellTotal() {
            return this.sellPrice * this.sellNum * 100;
        },
        // 券商佣金
        brokerageCommission() {
            const sum = ((this.buyTotal + this.sellTotal) * this.ratio) / 10000;
            return sum < 10 ? 10 : sum;
        },
        // 印花税
        stampDuty() {
            const sum = (this.sellTotal * this.stampDutyRate) / 100;
            return sum;
        },
        // 税费合计
        feesTotal() {
            return this.brokerageCommission + this.stampDuty;
        },
        // 投资收益
        earnings() {
            return this.sellTotal - this.buyTotal - this.feesTotal;
        },
        // 盈亏比例
        profitAndLossRatio() {
            return (this.earnings / this.buyTotal) * 100;
        },
    },
    // 方法
    methods: {
        submit() {},
        reset() {
            this.buyPrice = 0;
            this.buyNum = 1;
            this.sellPrice = 0;
            this.sellNum = 1;
            this.ratio = 2.5;
            this.stampDutyRate = 0.1;
        },
    },
    // 生命周期函数 请求写在created中,echarts写在mounted中
    created() {},
    beforeMount() {},
    mounted() {},
    beforeDestroy() {},
    destroyed() {},
    // 监视
    watch: {},
    // 组件
    components: {},
    // 过滤器
    filters: {},
    // 自定义指令
    directive: {},
};
</script>

<style lang="less" scoped>
.first-floor {
    padding: 10px 0 0;
    display: flex;
    justify-content: space-around;
}
.form {
    // width: 300px;

    label {
        display: inline-block;
        width: 100px;
    }
    // .form-info {
    //     display: inline-block;
    //     width: 30px;
    // }
}
.form-item {
    display: flex;
    // justify-content: space-between;
    padding: 2px 0;
}
.form-btn-box {
    display: flex;
    justify-content: center;
}
.ft16 {
    font-size: 16px;
}
</style>
