<template>
    <div class="category">
        <header>
            <i class="iconfont icon-fanhui"></i>
            <span>商品分类</span>
            <i class="iconfont icon-gengduo"></i>
        </header>
        <main>
            <div class="first-cate">
                <ul>
                    <li 
                    v-for="(item, index) in firstCateList" 
                    :key="item.id"
                    :class="{active: index == ind}"
                    @click="changeCate(index)">
                        {{ item.catename }}
                    </li>
                </ul>
            </div>
            <div class="second-cate">
                <ul v-if="firstCateList.length > 0">
                    <li
                    @click="jump(item.id)"
                     v-for="(item, index) in firstCateList[ind].children" :key="item.id">
                        <div class="img-box">
                        <img :src="'http://localhost:3000' + item.img" alt="">

                        </div>
                        <p>{{ item.catename}}</p>
                    </li>
                </ul>
            </div>
        </main>
    </div>
</template>

<script>
    export default {
        data() {
            return {
                firstCateList: [],
                ind: 0
            }
        },
        mounted() {
            this.getFirstCate()
        },
        methods: {
            getFirstCate() {
                this.$http.get("/getcatetree").then(res => {
                    console.log(res)
                    this.firstCateList = res.data.list
                })
            },
            changeCate(i) {
                // 1. 高亮切花 2. 右侧数据切换
                this.ind = i
            },
            jump(id) {
                this.$router.push("/home/index?id=" + id)
            }
        }
    }
</script>

<style lang="stylus" scoped>
.category 
    width 100%
    height 100%
    header 
        height .88rem
        background-color color
        color #fff
        display flex 
        justify-content space-between
        line-height .88rem
        padding 0 .3rem
    main 
        display flex
        .first-cate
            width 1.7rem
            li 
                height .5rem
                line-height .5rem
                margin .25rem 0
                padding-left .3rem
                border-left .06rem solid transparent
                &.active 
                    border-left .06rem solid color
        .second-cate
            padding-left .3rem
            flex 1
            ul 
                display flex
                flex-wrap wrap
                li 
                    width 30%
                    margin-right 3%
                    height 2rem
                    .img-box 
                        width 1.3rem
                        height 1.3rem


</style>