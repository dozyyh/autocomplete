# autocomplete
输入框历史记录

<template>
    <div class="container">
        <iframe name="stop" style="display: none;"></iframe>
        <el-form
            ref="form"
            :model="form"
            action="http://"
            target="stop"
            label-width="80px"
            :rules="rules"
            hide-required-asterisk
            @keyup.enter.native="onSubmit"
            style="width: 400px; margin: 20px auto"
        >
            <h1 style="text-align: center; margin-top: 20px">
                旺旺信息查询平台
            </h1>
            <el-form-item label="旺旺ID:" prop="name">
                <el-input
                    v-model.trim="form.name"
                    id="name"
                    size="small"
                    autocomplete="on"
                    style="width: 300px"
                ></el-input>
            </el-form-item>
            <el-form-item label="验证信息:" prop="passWord">
                <el-input
                    v-model.trim="form.passWord"
                    size="small"
                    style="width: 300px"
                ></el-input>
            </el-form-item>

            <el-form-item>
               
                <el-button type="primary" size="small" @click="onSubmit"
                    >查询</el-button
                >
            </el-form-item>
        </el-form>
    </div>
</template>
<script>
export default {
    name: "/business/manage",
    components: {},
    mixins: [],
    props: {},
    data() {
        return {
            form: {
                name: "",
                passWord: "",
            },
            rules: {
                name: [
                    {
                        required: true,
                        message: "请输入旺旺ID",
                    },
                ],
                passWord: [
                    {
                        required: true,
                        message: "请输入验证信息",
                    },
                ],
            },
        };
    },
    computed: {},
    watch: {},
    mounted() {},
    methods: {
        onSubmit() {
            this.$refs.form.validate((valid) => {
                if (valid) {
                    let params={
                        nick:this.form.name,
                        pswd:this.form.passWord,
                    }
                    this.$http
                        .businessBuyerLogin(params)
                        .then((res) => {
                            if(res.code==200){
                                this.$refs.form.$el.submit();
                                this.$message.success("操作成功");
                                localStorage.setItem("ACCESS-YLMF-TOKEN", res.data);
                                this.$store.commit("setToken", res.data); //context提交
                                this.$router.push("/home-page");
                            }else {
                                this.$message.error(res.msg)
                            }
                        })
                        .catch((err) => {
                            this.$message.error(err);
                        });
                }
            });
        },
    },
};
</script>
<style lang="less" scoped>

/deep/ .el-input__inner:-webkit-autofill {
    transition: background-color 50000s ease-in-out 0s;
    background: #fff;
}
</style>
