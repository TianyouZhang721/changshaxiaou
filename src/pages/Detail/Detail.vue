<template>
    <div class="detail">
        <header>
            <i class="iconfont icon-fanhui"></i>
            <span>商品详情</span>
            <i class="iconfont icon-gengduo"></i>
        </header>
        <main>
            <div class="img-box">
                <img :src="'http://localhost:3000' + detail.img" alt="">
            </div>
            <div class="content">
                <p>{{ detail.goodsname }}</p>
                <p>{{ detail.price }}</p>
            </div>
            <div class="specs">
                <div class="count-box">
                    <p>购买数量</p>
                    <div>
                        <button @click="jian">-</button>
                        <span>{{ count }}</span>
                        <button @click="jia">+</button>
                    </div>
                </div>
                <div class="specs-box">
                    <p>规格属性</p>
                    <div>
                        <span @click="changeSpec" :class="{active: flag}">{{ detail.specsattr }}</span>
                    </div>
                </div>
            </div>
        </main>
        <footer>
            <div class="cart" @click="goCart">
                <i class="iconfont icon-gouwuche"></i>
                <p>购物车</p>
            </div>
            <div class="btns">
                <button @click="addcart" class="addCart">加入购物车</button>
                <button class="buy">立即购买</button>
            </div>
        </footer>
    </div>
</template>

<script>
import { Toast } from 'vant';
    export default {
        data() {
            return {
                flag: false,
                detail: {},
                count: 1
            }
        },
        mounted() {
            this.$http.get("/getgoodsinfo", {id: this.$route.query.id}).then(res => {
                console.log(res)
                this.detail = res.data.list[0]
            })
        },
        methods: {
            goCart() {
                this.$router.push("/home/cart")
            },
            jian() {
                if (this.count == 1) {
                    return
                }
                this.count--
            },
            jia() {
                this.count++
            },
            changeSpec() {
                this.flag = !this.flag
            },
            addcart() {
                // 判断是否选中了规格
                if (!this.flag) {
                    Toast.fail('请选择一个规格');
                    return
                }
                // 判断登录状态,获取vuex中的user，如果user中有nickname则代表已经登录
                if (!this.user.nickname) {
                    Toast.fail('请登录,将在2秒之后自动跳到登录页');
                    setTimeout(() => {
                        this.$router.push("/login")
                    }, 2000)
                    return
                }

                // 通过两层验证，调用添加购物车接口
                this.$http.post("/cartadd",{
                    uid: this.user.uid,
                    goodsid: this.$route.query.id,
                    num: this.count
                }).then(res => {
                    console.log(res)
                })
            }
        },
        computed: {
            user() {
                return this.$store.state.user
            }
        }
    }
</script>

<style lang="stylus" scoped>
.detail 
    width 100%
    height 100%
    display flex 
    flex-direction column
    header 
        height .88rem
        display flex 
        justify-content space-between
        padding 0 .3rem;
        line-height .88rem
        background-color color 
        color #fff
    main    
        flex 1
        overflow auto
        padding 0 .3rem
        .count-box, .specs-box
            display flex 
            justify-content space-between
            margin .15rem 0
            div 
                button 
                    width .6rem
                    height .6rem
        .specs-box 
            span 
                &.active 
                    color color
        
    footer 
        height 1rem
        display flex 
        .cart 
            width 1rem
            height 100%
            text-align center
        .btns 
            flex 1
            display flex 
            button 
                flex 1
                color #fff
                &.addCart 
                    background-color color
                &.buy 
                    background-color #d16a32
                
</style>