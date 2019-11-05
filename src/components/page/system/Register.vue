<template>
    <div class="container">
        <el-container style="height: 600px;">
            <el-form :model="form" label-width="450px">
                <el-form-item label="用户名：">
                    <el-input v-model="form.account" @change="SearchAcc(form.account)" onkeyup="this.value=this.value.replace(/\s+/g,'')"></el-input>
                    <div v-if="SearchDiv=='error'">
                        <i class="el-icon-error" style="color:red;display:inline-block"></i>
                        <p style="color:red;display:inline-block">该用户名已存在</p>
                    </div>
                    <div v-if="SearchDiv=='success'">
                        <i class="el-icon-success" style="color:green;display:inline-block"></i>
                        <p style="color:green;display:inline-block">该用户名可以使用</p>
                    </div>
                </el-form-item>
                <el-form-item label="密码：">
                    <el-input v-model="form.password" onkeyup="this.value=this.value.replace(/[\u4e00-\u9fa5]|\s+/g,'')" style="display:inline-block"></el-input>
                </el-form-item>
                <el-form-item label="对应工号：">
                    <el-input v-model="form.gNum" onkeyup="this.value=this.value.replace(/[\u4e00-\u9fa5]|\s|[a-zA-Z]+/g,'')" style="width:95px;display:inline-block" @change="SearchGnum(form.gNum)"></el-input>
                    <el-input v-model="form.name" style="width:100px;display:inline-block" :disabled="true"></el-input>
                </el-form-item>
                <el-form-item>
                    <el-button type="primary" style="width:200px" @click="Displaymodule=true">显示模块权限设置</el-button>
                </el-form-item>
                <el-form-item>
                    <el-button type="danger" style="width:95px" @click="Reset()">重置</el-button>
                    <el-button type="primary" style="width:95px" @click="Register()">注册</el-button>
                </el-form-item>
            </el-form>
            <el-dialog  title="显示模块" :visible.sync="Displaymodule">
                <el-form :model="form">
                <el-tree 
                    :data="data" 
                    :props="defaultProps" 
                    show-checkbox 
                    ref="tree"
                    highlight-current
                    node-key="value"></el-tree>
                </el-form>
                <div slot="footer" class="dialog-footer">
                <el-button @click="Displaymodule = false ">取 消</el-button>
                <el-button type="primary" @click="Determine()">确 定</el-button>
                </div>
            </el-dialog>
        </el-container>
    </div>
</template>

<script>
import axios from 'axios'
export default {
    data(){
        return{
            form:{
                account:'',
                password:'',
                gNum:'',
                name:'',
                seeModule:'',
                cuser:'',
            },
            SearchDiv:'',
            Displaymodule:false,
            data: [
                {
                label:"系统首页",
                value:"1" 
                },
                {
                label: "制造模块",
                value:"13",
                children: [
                    {
                    label: "车间计划",
                    value:"15"
                    },
                    {
                    label: "电子看板",
                    value:"16"
                    },
                    {
                    label: "设备安全点检",
                    value:"17"
                    },
                    {
                    label: "在建项目",
                    value:"14",
                    children:[
                        {
                        label:"查看在建项目模块",
                        value:"14"
                        },
                        {
                        label:"导入在建项目",
                        value:"14_DRZJ"
                        },
                        {
                        label:"删除在建项目",
                        value:"14_SCZJ"
                        }
                    ]
                    },
                    {
                    label: "装配出仓",
                    value:"31",
                    children:[
                        {
                        label:"查看装配出仓模块",
                        value:"31"
                        },
                        {
                        label:"新建装配清单",
                        value:"31_XJZP"
                        },
                        {
                        label:"修改装配清单",
                        value:"31_XGZP"
                        },
                        {
                        label:"删除装配清单",
                        value:"31_SCZP"
                        }
                    ]
                    }
                ]
                },
                {
                label: "工艺模块",
                value:"18",
                children: [
                    {
                    label: "工艺卡管理",
                    value:"19",
                    children:[
                        {
                        label:"查看工艺卡模块",
                        value:"19"
                        },
                        {
                        label:"新建工艺卡",
                        value:"19_XJGYK"
                        },
                        {
                        label:"删除工艺卡",
                        value:"19_SCGYK"
                        },
                        {
                        label:"修改工艺卡",
                        value:"19_XGGYK"
                        },
                        {
                        label:"复制工艺卡",
                        value:"19_FZGYK"
                        }
                    ]
                    },{
                    label: "已完成项目",
                    value:"21"
                    }
                ]
                },{
                label: "检验模块",
                value:"22"
                },
                {
                label: "工时统计",
                value:"23"
                },
                {
                label: "系统设置",
                value:"24",
                children: [
                    {
                    label: "员工信息管理",
                    value:"25"
                    },
                    {
                    label: "操作日志",
                    value:"26"
                    },
                    {
                    label: "权限管理",
                    value:"27"
                    },
                    {
                    label: "数据归档",
                    value:"29",
                    children:[
                        {
                        label:"查看数据归档模块",
                        value:"29"
                        },
                        {
                        label:"导出归档文件",
                        value:"29_DCGD"
                        }
                    ]
                    },
                    {
                        label: "注册新用户",
                        value:"32",
                        }
                ]
                }
            ],
            defaultProps: {
                children: "children",
                label: "label"
            },
        }
    },
    methods:{
        //查重用户名
        SearchAcc(acc){
            // console.log(acc);
            if(acc==''||acc==null){
                 this.SearchDiv=''
            }else{
                var fd = new FormData();
                fd.append('flag','Searchacc')
                fd.append('acc',acc)
                axios.post(`${this.baseURL}/system/Adduser.php`,fd).then((res)=>{
                    this.SearchDiv=res.data.success
                })
            }
        },
        //匹配工号
        SearchGnum(gNum){
            if(gNum==''||gNum==null){
                this.form.name = ''
            }else{
                var fd = new FormData();
                fd.append('flag','SearchGnum')
                fd.append('gNum',gNum)
                axios.post(`${this.baseURL}/system/Adduser.php`,fd).then((res)=>{
                    if(res.data.success=='success'){
                        this.form.name = res.data.name
                    }else{
                        this.form.name = '工号不存在'
                    }
                })
            }
        },
        //确定权限
        Determine(){
            this.form.seeModule = this.$refs.tree.getCheckedKeys()
            // console.log(this.seeModule)
            this.Displaymodule = false
        },
        Reset(){
            this.SearchDiv=''
            this.form.account=''
            this.form.password=''
            this.form.gNum=''
            this.form.name=''
            this.form.seeModule=''
        },
        //注册
        Register(){
            if(this.SearchDiv=='error'||this.form.name=='工号不存在'||this.SearchDiv==''||this.form.name==''){
                this.$message.error('注册失败,请注意内容填写！')
            }else{
                this.form.cuser = localStorage.getItem('account')
                var fd = new FormData();
                fd.append('flag','Register')
                fd.append('account',this.form.account)
                fd.append('password',this.form.password)
                fd.append('gNum',this.form.gNum)
                fd.append('seeModule',this.form.seeModule)
                fd.append('cuser',this.form.cuser)
                axios.post(`${this.baseURL}/system/Adduser.php`,fd).then((res)=>{
                    if(res.data.success=='success'){
                        this.$message({
                            message: '注册成功！',
                            type: 'success'
                        });
                        this.Reset()
                    }else{
                        this.$message.error('注册失败,请联系管理员！');
                    }
                })
            }
        }
    }
}
</script>