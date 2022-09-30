<template>
    <el-container>
        <el-main>
            <el-form ref="form" :model="form" label-width="80px">
                <el-form-item label="账号">
                    <el-input v-model="form.username"></el-input>
                </el-form-item>
                <el-form-item label="密码">
                    <el-input v-model="form.password"></el-input>
                </el-form-item>
                <el-form-item>
                    <el-button type="primary" @click="onSubmit">立即登录</el-button>
                </el-form-item>
            </el-form>
        </el-main>
    </el-container>
</template>

<script>
    export default {
        name: "LoginView",
        data() {
            return {
                form: {
                }
            }
        },
        methods: {
            onSubmit() {
                this.axios.post('http://localhost:9001/login/user/login',this.form).then(res =>{
                    if (res.data.data == 200){
                        this.$message.success('登录成功')
                        this.$store.commit('setUserId',res.data.code)
                        this.$router.push('ListView')
                    }else {
                        this.$message.error(res.data.data)
                    }
                })
            }
        }
    }
</script>

<style scoped>

</style>
