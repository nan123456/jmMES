<template>
    <div class="login-wrap">
        <div class="ms-login">
            <div class="ms-title">中山金马科技MES系统</div>
            <div class="ms-from">
                <el-form :model="ruleForm" :rules="rules" ref="ruleForm" label-width="0px" class="demo-ruleForm">
                    <el-form-item prop="username">
                        <el-input v-model="ruleForm.username" placeholder="username"></el-input>
                    </el-form-item>
                    <el-form-item prop="password">
                        <el-input type="password" placeholder="password" v-model="ruleForm.password" @keyup.enter.native="submitForm('ruleForm')"></el-input>
                    </el-form-item>
                    <div class="login-btn">
                        <el-button type="primary" @click="submitForm()">登录</el-button>
                    </div>
                    <!-- <p style="font-size:12px;line-height:30px;color:#999;">Tips : 用户名和密码随便填。</p> -->
                </el-form>
            </div>
        </div>
    </div>
</template>

<script>
    import axios from "axios"
    export default {
        data: function(){
            return {
                ruleForm: {
                    username: '',
                    password: '',
                    department: ''
                },
                rules: {
                    username: [
                        { required: true, message: '请输入用户名', trigger: 'blur' }
                    ],
                    password: [
                        { required: true, message: '请输入密码', trigger: 'blur' }
                    ]
                }
            }
        },
        methods: {
            submitForm() {
                 var fd = new FormData()
                    fd.append("flag","Login")
                    fd.append("username",this.ruleForm.username)
                    fd.append("password",this.ruleForm.password)
                    axios.post(`${this.baseURL}/login.php`,fd).then((res)=> {
                        console.log(res.data)
                        localStorage.setItem('ms_username',this.ruleForm.username);
                        localStorage.setItem('ms_department',res.data.department);
                        if(res.data.status == "success"){
                            this.$router.push('/dashboard');
                        }else{
                            alert("登录失败")
                        }
                        
                    })
               
               
            }
        }
    }
</script>

<style scoped>
    .login-wrap{
        /* position: relative; */
        display: flex;
        align-content: center;
        width:100%;
        height:100%;
        background-size:cover; 
        background-image: url("../../assets/background.png");
    }
    .ms-title{
        /* position: absolute;
        top:50%;
        width:100%;
        margin-top: -230px;
        text-align: center;
        font-size:30px;
        color: #fff; */
        position: relative;
        margin-top: 0px;
        text-align: center;
        font-family: "Myriad Pro","Helvetica Neue",Arial,Helvetica,sans-serif;
        font-weight: 600;
        font-size:29px;
        color: #0050b3;

    }
    .ms-login{
        position: absolute;
        left:50%;
        top:45%;
        width:300px;
        height:250px;
        margin:-150px 0 0 -190px;
        padding:40px;
        border-radius: 5px;
        background: rgba(255, 255, 255, 0.8);
    }
    .ms-from{
        position: relative;
        margin-top: 50px;
    }
    .login-btn{
        text-align: center;
    }
    .login-btn button{
        width:100%;
        height:36px;
    }
</style>