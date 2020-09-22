<template>
    <div class="index">
        <div class="home-index"
             v-if="!id">
            <header class="pd">
                <div class="logo-box">
                    <img src="../../assets/logo.jpg"
                         alt="">
                </div>
                <input type="text">
                <i class="iconfont icon-gengduo"></i>
            </header>
            <div class="banner">
                <div class="swiper-container">
                    <div class="swiper-wrapper">
                        <div class="swiper-slide"
                             v-for="(item, index) in bannerList"
                             :key="item.id">
                            <img :src="'http://localhost:3000' + item.img"
                                 alt="">
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
                    <span :class="{active: index == ind}"
                          @click="change(index)"
                          v-for="(item, index) in navList"
                          :key="index">{{ item }}</span>
                </div>
                <!-- 数据列表 -->
                <GoodsList v-if="goodsList.length > 0"
                           :goodsList="goodsList[ind].content" />
            </div>
        </div>
        <div class="home-list"
             v-if="id">
            <header>
                <i class="iconfont icon-fanhui"></i>
                <span>商品列表</span>
                <i class="iconfont icon-gengduo"></i>
            </header>
            <main>
                <div class="search-box">
                    <input type="text" placeholder="搜索商品">
                    <i class="iconfont icon-icon-"></i>
                </div>
                <div class="goods-list">
                    <ul>
                        <li v-for="(item, index) in cateGoodsList" :key="item.id">
                            <div class="img-box">
                                <img :src="'http://localhost:3000' + item.img"
                                     alt="">
                            </div>
                            <div class="content">
                                <p>{{ item.goodsname }}</p>
                                <p>{{ item.price }}</p>
                            </div>
                        </li>
                    </ul>
                </div>
            </main>
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
    data () {
        return {
            ind: 0,
            navList: ["最热商品", "最新商品", "全部商品"],
            bannerList: [],
            goodsList: [],
            id: "",
            cateGoodsList: []
        }
    },
    watch: {
        $route: {
            deep: true,
            handler(newVal) {
                console.log(newVal)
                this.id = newVal.query.id
            }
        }
    },
    // beforeRouteUpdate() {}
    mounted () {
        console.log(this.$route)
        // 获取路由参数
        this.id = this.$route.query.id
        this.getGoodsList()
        this.getCateGoodsList()
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
    // beforeRouteUpdate(to, from ,next) {
    //     console.log(to)
    //     this.id = to.query.id
    //     next()
    // },
    // watch: {
    //     $route
    // },
    methods: {
        getCateGoodsList() {
            this.$http.get("/getgoods", {fid: this.id}).then(res => {
                console.log(res)
                this.cateGoodsList = res.data.list
            })
        },
        change (i) {
            // 1. 高亮切换 2. 数据切换
            this.ind = i
        },
        getGoodsList () {
            this.$http.get("/getindexgoods").then(res => {
                this.goodsList = res.data.list
            })
        },
        goCategory () {
            this.$router.push("/category")
        }
    },
}
</script>

<style lang="stylus" scoped>
.index
    width 100%
    height 100%
    .home-index
        header
            width 100%
            height 0.88rem
            display flex
            justify-content space-between
            .logo-box
                width 1.5rem
                height 0.4rem
            input
                width 3rem
                height 0.4rem
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
                height 0.66rem
                line-height 0.66rem
                span
                    flex 1
                    text-align center
                    border 0.01rem solid #ccc
                    &.active
                        background-color color
                        color #fff
    .home-list
        header 
            height .88rem
            background-color #fff
            display flex
            justify-content space-between
            padding 0 .3rem
            line-height .88rem
        main
            .goods-list
                li 
                    display flex
                    height 2rem
                    .img-box 
                        width 1.7rem
                    .content
                        flex 1
            .search-box
                width 6.2rem
                height .8rem
                margin .3rem auto
                position relative
                input 
                    width 100%
                    height 100%
                    border-radius .4rem
                    background-color color
                    text-align center
                    text-indent .3rem
                    &::-webkit-input-placeholder
                        color #fff
                i 
                    position absolute
                    left 2.3rem
                    top 50%
                    transform translateY(-50%)
                    color #ffffff
                    font-size .4rem
</style>