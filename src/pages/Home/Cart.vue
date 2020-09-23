<template>
    <div class="cart">
        <header>
            <i class="iconfont icon-fanhui"></i>
            <span>购物车</span>
            <i class="iconfont icon-gengduo"></i>
        </header>
        <main>
            <ul>
                <li ref="lis" v-for="(item,index) in cartList" :key="item.id">
                    <div class="content" @touchstart="start" @touchend="end(index, $event)">
                        <i @click="checkOne(index)" :class="['iconfont','icon-duoxuan', {active: item.isChecked}]"></i>
                        <div class="img-box">
                            <img :src="'http://localhost:3000' + item.img" alt="">
                        </div>
                        <div class="goods-title">
                            <p>{{ item.goodsname }}</p>
                            <p>{{ item.price}}</p>
                        </div>
                        <div class="count-box">
                            <button @click="jian(index)">-</button>
                            <span>{{ item.num }}</span>
                            <button @click="jia(index)">+</button>
                        </div>
                    </div>
                    <div class="del" @click="del(item.id)">删除</div>
                </li>
            </ul>
        </main>
        <footer>
            <div class="checkAll-box">
                <i @click="checkAll" :class="['iconfont','icon-duoxuan', {active: isCheckAll}]"></i>
                全选
            </div>
            <div class="total-box">
                <div class="total-price">
                    总计：{{ totalPrice}}元
                </div>
                <div class="total-count">
                    去结算({{totalCount}}件)
                </div>
            </div>
        </footer>
    </div>
</template>

<script>
    import { mapState } from 'vuex'
    export default {
        data() {
            return {
                cartList: [],
                isCheckAll: false,
                totalCount: 0,
                totalPrice: 0,
                startX: ""
            }
        },
        watch: {
            cartList: {
                deep: true,
                handler(newVal) {
                    // 全选
                    this.isCheckAll = newVal.every(item => {
                        return item.isChecked
                    })
                    // 筛选出所有选中的
                    let arr = newVal.filter(item => {
                        return item.isChecked
                    })
                    this.totalCount = arr.length
                    this.totalPrice = 0;
                    arr.forEach(item => {
                        this.totalPrice += item.num * item.price
                    })
                }
            }
        },
        methods: {
            del(id) {
                this.$http.post("/cartdelete", {id}).then(res => {
                    console.log(res)
                })
            },
            // 手指触碰屏幕的事件
            start(e) {
                console.log(e)
                this.startX = e.touches[0].clientX
            },
            // 手指从屏幕离开的事件
            end(i, e) {
                console.log(this.$refs.lis)
                var endX = e.changedTouches[0].clientX
                if (this.startX - endX > 15) {
                    console.log("左滑")
                    // 将整个li进行移动
                    this.$refs.lis[i].style.transform = "translateX(-1rem)"
                }
                if (this.startX - endX < 15) {
                    console.log("右滑")
                    // 将整个li进行移动
                    this.$refs.lis[i].style.transform = "translateX(0rem)"
                }
            },
            jian(i) {
                if (this.cartList[i].num == 1) {
                    return
                }
                this.cartList[i].num--
            },
            jia(i) {
                this.cartList[i].num++
            },
            checkOne(i) {
                // 切换点击的商品的全选变色
                this.cartList[i].isChecked = !this.cartList[i].isChecked
            },
            checkAll() {
                this.isCheckAll = !this.isCheckAll
                this.cartList.forEach(item => {
                    item.isChecked = this.isCheckAll
                })
            }
        },
        mounted() {
            this.$http.get("/cartlist", {uid: this.user.uid}).then(res => {
                this.cartList = res.data.list.map(item => {
                    return {...item, isChecked: false}
                })
                console.log(this.cartList)
            })
        },
        computed: {
            ...mapState(["user"])
        }
    }
</script>

<style lang="stylus" scoped>
.cart 
    width 100%
    height 100%
    display  flex
    flex-direction column
    header 
        height .88rem
        background-color color
        color #fff
        display flex 
        justify-content space-between
        padding 0 .3rem
        line-height .88rem
    main 
        flex 1
        overflow auto
        ul 
            overflow hidden
            li 
                width 100%;
                height 1.7rem
                position relative
                transition transform 1s
                .content 
                    width 100%
                    display flex
                    justify-content space-between
                    align-items center
                    padding 0 .3rem
                    i 
                        font-size .46rem
                        &.active 
                            color color
                    .img-box
                        width 1.3rem
                        height 1.3rem
                    button 
                        width .6rem
                        height .5rem
                .del 
                    width 1rem
                    height 1.7rem
                    background-color darkred
                    color #fff
                    text-align center
                    line-height 1.7rem
                    position absolute
                    top 0
                    right -1rem
    footer 
        height 1rem
        display flex
        text-align center
        line-height 1rem
        .checkAll-box
            width 2rem
            i 
                &.active 
                    color color
        .total-box
            flex 1
            display flex
            div 
                flex 1
                &.total-count
                    background-color color
                    color #fff
</style>