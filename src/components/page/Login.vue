<template>
    <div class="login-wrap">
        <div class="ms-login">
            <div class="ms-title">中山金马科技MES系统</div>
            <div class="ms-from">
                <el-form :model="ruleForm" :rules="rules" ref="ruleForm" label-width="0px" class="demo-ruleForm">
                    <el-form-item prop="username">
                        <el-input v-model="ruleForm.username" placeholder="用户名"></el-input>
                    </el-form-item>
                    <el-form-item prop="password">
                        <el-input type="password" placeholder="密码" v-model="ruleForm.password" @keyup.enter.native="submitForm('ruleForm')"></el-input>
                    </el-form-item>
                    <el-checkbox v-model="checked" @change="remember()">记住密码</el-checkbox>

                    <div class="login-btn">
                        <el-button type="primary" @click="submitForm()">登录</el-button>
                    </div>
                </el-form>
            </div>
        </div>
    </div>
</template>

<script>
    import axios from "axios"
    import { Base64 } from 'js-base64';
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
                },
                checked:false
            }
        },
        created() {
            this.ruleForm.username=localStorage.account;
            this.ruleForm.password=Base64.decode(localStorage.password);
            if(localStorage.password!=""){
                this.checked=true;
            }
        },
        methods: {
            submitForm() {
                if(this.checked==true){
                    localStorage.account=this.ruleForm.username;
                    localStorage.password=Base64.encode(this.ruleForm.password);
                }else if(this.checked==false){
                    localStorage.account='';
                    localStorage.password='';
                }
                 var fd = new FormData()
                    fd.append("flag","Login")
                    fd.append("username",this.ruleForm.username)
                    fd.append("password",this.ruleForm.password)
                    axios.post(`${this.baseURL}/login.php`,fd).then((res)=> {
                        // console.log(res.data)
                        localStorage.setItem('ms_username',this.ruleForm.username);
                        localStorage.setItem('ms_department',res.data.department);
                        // localStorage.setItem('seeModule',res.data.seeModule);
                        localStorage.setItem('userid',res.data.id);
                        if(res.data.status == "success"){
                            this.$router.push('/dashboard');
                        }else{
                            alert("登录失败")
                        }
                        
                    })
               
               
            },

            encodeUTF8(s) {
                var i, r = [], c, x;
                for (i = 0; i < s.length; i++)
                    if ((c = s.charCodeAt(i)) < 0x80) r.push(c);
                    else if (c < 0x800) r.push(0xC0 + (c >> 6 & 0x1F), 0x80 + (c & 0x3F));
                    else {
                    if ((x = c ^ 0xD800) >> 10 == 0) //对四字节UTF-16转换为Unicode
                        c = (x << 10) + (s.charCodeAt(++i) ^ 0xDC00) + 0x10000,
                        r.push(0xF0 + (c >> 18 & 0x7), 0x80 + (c >> 12 & 0x3F));
                    else r.push(0xE0 + (c >> 12 & 0xF));
                    r.push(0x80 + (c >> 6 & 0x3F), 0x80 + (c & 0x3F));
                    };
                return r;
            },

            // 字符串加密成 hex 字符串
            sha1(s) {
                var data = new Uint8Array(this.encodeUTF8(s))
                var i, j, t;
                var l = ((data.length + 8) >>> 6 << 4) + 16, s = new Uint8Array(l << 2);
                s.set(new Uint8Array(data.buffer)), s = new Uint32Array(s.buffer);
                for (t = new DataView(s.buffer), i = 0; i < l; i++)s[i] = t.getUint32(i << 2);
                s[data.length >> 2] |= 0x80 << (24 - (data.length & 3) * 8);
                s[l - 1] = data.length << 3;
                var w = [], f = [
                    function () { return m[1] & m[2] | ~m[1] & m[3]; },
                    function () { return m[1] ^ m[2] ^ m[3]; },
                    function () { return m[1] & m[2] | m[1] & m[3] | m[2] & m[3]; },
                    function () { return m[1] ^ m[2] ^ m[3]; }
                ], rol = function (n, c) { return n << c | n >>> (32 - c); },
                    k = [1518500249, 1859775393, -1894007588, -899497514],
                    m = [1732584193, -271733879, null, null, -1009589776];
                m[2] = ~m[0], m[3] = ~m[1];
                for (i = 0; i < s.length; i += 16) {
                    var o = m.slice(0);
                    for (j = 0; j < 80; j++)
                    w[j] = j < 16 ? s[i + j] : rol(w[j - 3] ^ w[j - 8] ^ w[j - 14] ^ w[j - 16], 1),
                        t = rol(m[0], 5) + f[j / 20 | 0]() + m[4] + w[j] + k[j / 20 | 0] | 0,
                        m[1] = rol(m[1], 30), m.pop(), m.unshift(t);
                    for (j = 0; j < 5; j++)m[j] = m[j] + o[j] | 0;
                };
                t = new DataView(new Uint32Array(m).buffer);
                for (var i = 0; i < 5; i++)m[i] = t.getUint32(i << 2);

                var hex = Array.prototype.map.call(new Uint8Array(new Uint32Array(m).buffer), function (e) {
                    return (e < 16 ? "0" : "") + e.toString(16);
                }).join("");
                return hex;
            },
            remember(){
                if(this.checked==false){
                    localStorage.account='';
                    localStorage.password='';
                }
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
        background-image: url("http://47.106.161.130:80/img/background.png");
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
        margin-top: 15px;
    }

</style>