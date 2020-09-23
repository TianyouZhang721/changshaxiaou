<template>
    <div class="login">
        <input type="text" v-model="phone">
        <br>
        <input type="text" v-model="nickname">
        <br>
        <input type="text" v-model="password">
        <br>
        <button @click="register">注册</button>
    </div>
</template>

<script>
    export default {
        data() {
            return {
                phone: "",
                nickname: "",
                password: ""
            }
        },
        methods: {
            register() {
                this.$http.post("/register", {
                    phone: this.phone,
                    nickname: this.nickname,
                    password: this.password
                }).then(res => {
                    console.log(res)
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