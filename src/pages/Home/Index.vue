<template>
    <div class="index">
        <header class="pd">
            <div class="logo-box">
                <img src="../../assets/logo.jpg" alt="">
            </div>
            <input type="text">
            <i class="iconfont icon-gengduo"></i>
        </header>
        <div class="banner">
            <div class="swiper-container">
                <div class="swiper-wrapper">
                    <div 
                    class="swiper-slide"
                    v-for="(item, index) in bannerList" :key="item.id"
                    >
                        <img :src="'http://localhost:3000' + item.img" alt="">
                    </div>
                </div>
            </div>
        </div>
        <nav>
            <div>
                <i class="iconfont icon-icon-test"></i>
                <p>限时抢购</p>
            </div>
            <div>
                <i class="iconfont icon-shangcheng"></i>
                <p>积分商城</p>
            </div>
            <div>
                <i class="iconfont icon-lianxiwomen"></i>
                <p>联系我们</p>
            </div>
            <div @click="goCategory">
                <i class="iconfont icon-leimupinleifenleileibie"></i>
                <p>商品分类</p>
            </div>
        </nav>
        <div class="tab">
            <!-- 点击tab切换 -->
            <div class="tab-box">
                <span 
                :class="{active: index == ind}"
                @click="change(index)"
                v-for="(item, index) in navList" 
                :key="index">{{ item }}</span>
            </div>
            <!-- 数据列表 -->
            <GoodsList v-if="goodsList.length > 0" :goodsList="goodsList[ind].content" />
        </div>
    </div>
</template>

<script>
    import Swiper from 'swiper'
    import GoodsList from '@/components/GoodsList'
    export default {
        components: {
            GoodsList
        },
        data() {
            return {
                ind: 0,
                navList: ["最热商品", "最新商品", "全部商品"],
                bannerList: [],
                goodsList: []
            }
        },
        mounted() {
            this.getGoodsList()
            this.$http.get("/getbanner").then(res => {
                console.log(res)
                this.bannerList = res.data.list
                //  this.$nextTick(() => {})
                // 当数据发生改变，会发生页面重新渲染，而页面渲染也是异步操作，我们现在需要保证后续代码要在页面渲染完成后再执行
                this.$nextTick(() => {
                    let myswiper = new Swiper(".swiper-container", {
                        autoplay: {
                            delay: 2000
                        }
                    })
                })
            })           
        },
        methods: {
            change(i) {
                // 1. 高亮切换 2. 数据切换
                this.ind = i
            },
            getGoodsList() {
                this.$http.get("/getindexgoods").then(res => {
                    console.log(res)
                    this.goodsList = res.data.list
                })
            },
            goCategory() {
                this.$router.push("/category")
            }
        },
    }
</script>

<style lang="stylus" scoped>
.index 
    width 100%
    height 100%
    header 
        width 100%
        height .88rem
        display flex 
        justify-content space-between
        .logo-box 
            width 1.5rem
            height .4rem
        input 
            width 3rem
            height .4rem
            background-color #eee
    .banner 
        width 100%
        height 2.9rem
        .swiper-container, .swiper-wrapper, .swiper-slide 
            width 100%
            height 100%
    nav 
        display flex 
        height 1.18rem
        align-items center
        div 
            flex 1
            text-align center
    .tab
        .tab-box
            display flex 
            height .66rem
            line-height .66rem
            span 
                flex 1
                text-align center
                border .01rem solid #ccc
                &.active 
                    background-color color
                    color #fff
</style>