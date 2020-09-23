<template>
    <div class="login">
        
        <input type="text" v-model="phone">
        <br>
        <input type="text" v-model="password">
        <br>
        <button @click="login">登录</button>
        <router-link to="/register">没有账号去注册</router-link>
    </div>
</template>

<script>
    export default {
        data() {
            return {
                phone: "",
                password: ""
            }
        },
        methods: {
            login() {
                this.$http.post("/login", {
                    phone: this.phone,
                    password: this.password
                }).then(res => {
                    console.log(res)
                    if (res.data.code == 200) {
                        // 调用mutations的方法
                        this.$store.commit("setUser", res.data.list)
                        this.$router.back()
                    }
                })
            }
        },
    }
</script>

<style lang="stylus" scoped>
.login 
    text-align center
input 
    width 3.2rem
    height .8rem
    background-color #eee
    margin .3rem 0
button 
    width 1rem 
    height .8rem
    background-color color
    color #fff

</style>