<template>
    <div class="sidebar">
        <!-- el-menu 参数 router 为是否使用 vue-router 的模式，启用该模式会在激活导航时以 index 作为 path 进行路由跳转-->
        <el-menu class="sidebar-el-menu" :default-active="onRoutes" :collapse="collapse" background-color="#324157"
            text-color="#bfcbd9" active-text-color="#20a0ff" unique-opened router>
            <template v-for="item in items">
                <template v-if="item.subs">
                    <el-submenu :index="item.index" :key="item.index">
                        <template slot="title">
                            <i :class="item.icon"></i><span slot="title">{{ item.title }}</span>
                        </template>
                        <el-menu-item v-for="(subItem,i) in item.subs" :key="i" :index="subItem.index">
                            {{ subItem.title }}
                        </el-menu-item>
                    </el-submenu>
                </template>
                <template v-else>
                    <el-menu-item :index="item.index" :key="item.index">
                        <i :class="item.icon"></i><span slot="title">{{ item.title }}</span>
                    </el-menu-item>
                </template>
            </template>
            <div class="Other-btn">
                 <!-- 消息中心 -->
                <div class="btn-bell">
                    <el-tooltip effect="dark" :content="message?`有${message}条未读消息`:`消息中心`" placement="bottom">
                        <router-link to="/tabs">
                            <i class="el-icon-bell"></i>
                        </router-link>
                    </el-tooltip>
                    <span class="btn-bell-badge" v-if="message"></span>
                </div>
                <!-- 全屏显示 -->
                <div class="btn-fullscreen" @click="handleFullScreen">
                    <el-tooltip effect="dark" :content="fullscreen?`取消全屏`:`全屏`" placement="bottom">
                        <i class="el-icon-rank"></i>
                    </el-tooltip>
                </div>
                <!-- 用户头像 -->
                <div class="user-avator">
                    <img src="static/img/img.jpg">
                    <el-dropdown class="user-name" trigger="click" @command="handleCommand" v-if="collapse==false">
                        <span class="el-dropdown-link">
                            {{username}} <i class="el-icon-caret-bottom"></i>
                        </span>
                        <el-dropdown-menu slot="dropdown">
                            <!-- <a href="http://blog.gdfengshuo.com/about/" target="_blank">
                                <el-dropdown-item>关于作者</el-dropdown-item>
                            </a> -->
                            <!-- <a href="https://github.com/lin-xin/vue-manage-system" target="_blank"> -->
                                <!-- <el-dropdown-item>
                                    <router-link to = "/personal_settings">个人设置</router-link>
                                </el-dropdown-item> -->
                            <!-- </a> -->
                            <el-dropdown-item divided  command="loginout">退出登录</el-dropdown-item>
                        </el-dropdown-menu>
                    </el-dropdown>
                </div>
            </div>
        </el-menu>
    </div>
</template>

<script>
    import axios from "axios";
    import bus from '../common/bus';
    export default {
        data() {
            return {
                collapse: false,
                items: [],
                name: 'jinma',
                fullscreen: false,
                message: 0,
            }
        },
        computed:{
            onRoutes(){
                return this.$route.path.replace('/','');
            },
            username(){
                let username = localStorage.getItem('ms_username');
                return username ? username : this.name;
            }
        },
        created(){
            // 通过 Event Bus 进行组件间通信，来折叠侧边栏
            bus.$on('collapse', msg => {
                this.collapse = msg;
            })

            this.getDataInfo();
        },
        methods:{
            // 用户名下拉菜单选择事件
            handleCommand(command) {
                if(command == 'loginout'){
                    localStorage.removeItem('ms_username')
                    this.$router.push('/login');
                }
            },
            // 全屏事件
            handleFullScreen(){
                let element = document.documentElement;
                if (this.fullscreen) {
                    if (document.exitFullscreen) {
                        document.exitFullscreen();
                    } else if (document.webkitCancelFullScreen) {
                        document.webkitCancelFullScreen();
                    } else if (document.mozCancelFullScreen) {
                        document.mozCancelFullScreen();
                    } else if (document.msExitFullscreen) {
                        document.msExitFullscreen();
                    }
                } else {
                    if (element.requestFullscreen) {
                        element.requestFullscreen();
                    } else if (element.webkitRequestFullScreen) {
                        element.webkitRequestFullScreen();
                    } else if (element.mozRequestFullScreen) {
                        element.mozRequestFullScreen();
                    } else if (element.msRequestFullscreen) {
                        // IE11
                        element.msRequestFullscreen();
                    }
                }
                this.fullscreen = !this.fullscreen;
            },
            getDataInfo(){
                var storage=window.localStorage
                var account=storage.ms_username
                var fd = new FormData()
                fd.append("flag","Sidebar")
                fd.append("account",account)
                axios.post(`${this.baseURL}/common/sidebar.php`,fd).then((res)=> {  //ES6写法
                    let retData = res.data;
                    if(retData.success == "success"){
                        this.items = JSON.parse(retData.data) 
                    }
                    // this.items = JSON.parse() 
                    // this.items = [ {
                    //     icon: 'el-icon-setting',
                    //     index: 'dashboard',
                    //     title: '系统首页'
                    // },
                    // {
                    //     icon: 'el-icon-more',
                    //     index: '2',
                    //     title: '基础数据管理',
                    //     subs: [
                    //         {
                    //             index: 'staff',
                    //             title: '员工信息管理'
                    //         },
                    //         {
                    //             index: 'equipment',
                    //             title: '设备管理'
                    //         },
                    //         {
                    //             index: 'document',
                    //             title: '文档管理'
                    //         },
                    //         {
                    //             index: 'material',
                    //             title: '原材料管理'
                    //         }
                    //     ]
                    // },
                    // {
                    //     icon: 'el-icon-message',
                    //     index: 'tabs',
                    //     title: '消息通知'
                    // },
                    // {   
                    //     icon: 'el-icon-tickets',
                    //     index: '4',
                    //     title: '销售模块',
                    //     subs: [
                    //         {
                    //             index: 'marketunreview',
                    //             title: '未审核项目'
                    //         },
                    //         {
                    //             index: 'market',
                    //             title: '在建项目'
                    //         },
                    //         {
                    //             index: 'marketfinished',
                    //             title: '已完成项目'
                    //         },
                    //         {
                    //             index: 'marketcheck',
                    //             title: '订单管理'
                    //         }
                    //     ]
                    // },
                    // {   
                    //     icon: 'el-icon-tickets',
                    //     index: '5',
                    //     title: '计划模块',
                    //     subs: [
                    //         {
                    //             index: 'planunreview',
                    //             title: '未审核项目'
                    //         },
                    //         {
                    //             index: 'plan',
                    //             title: '在建项目'
                    //         },
                    //         {
                    //             index: 'planfinished',
                    //             title: '已完成项目'
                    //         },
                    //         {
                    //             index: 'plancheck',
                    //             title: '订单查看'
                    //         },
                    //         {
                    //             index: 'plantable',
                    //             title: '计划总表'
                    //         }
                    //     ]
                    // },
                    // {   
                    //     icon: 'el-icon-tickets',
                    //     index: '6',
                    //     title: '工艺模块',
                    //     subs: [
                    //         {
                    //             index: 'craftunreview',
                    //             title: '未审核项目'
                    //         },
                    //         {
                    //             index: 'craft',
                    //             title: '在建项目'
                    //         },
                    //         {
                    //             index: 'craftfinished',
                    //             title: '已完成项目'
                    //         }
                    //     ]
                    // },
                    // {   
                    //     icon: 'el-icon-date',
                    //     index: '7',
                    //     title: '制造模块',
                    //     subs: [
                    //         {
                    //             index: 'electronic',
                    //             title: '电子看板'
                    //         },
                    //         {
                    //             index: 'productionplan',
                    //             title: '车间计划'
                    //         }
                    //     ]
                    // },
                    // {
                    //     icon: 'el-icon-search',
                    //     index: 'examine',
                    //     title: '检验模块'
                    // },
                    // {
                    //     icon: 'el-icon-star-on',
                    //     index: 'charts',
                    //     title: '统计分析'
                    // },
                    // {
                    //     icon: 'el-icon-setting',
                    //     index: '9',
                    //     title: '系统设置',
                    //     subs: [
                    //         {
                    //             index: 'operationlog',
                    //             title: '操作日志'
                    //         },
                    //         {
                    //             index: 'data',
                    //             title: '数据备份与还原'
                    //         },{
                    //             index: 'workshop',
                    //             title: '车间管理'
                    //         },
                    //         {
                    //             index: 'authority',
                    //             title: '权限管理'
                    //         },
                    //         {
                    //             index: 'cloudinteraction',
                    //             title: '云平台交互'
                    //         }
                    //     ]
                    // }];
                    // this.items = res.data;
                    // this.items = [ {
                    //     icon: 'el-icon-more',
                    //     index: '2',
                    //     title: '基础数据管理',
                    //     subs: [
                    //         {
                    //             index: 'staff',
                    //             title: '员工信息管理'
                    //         },
                    //         {
                    //             index: 'equipment',
                    //             title: '设备管理'
                    //         },
                    //         {
                    //             index: 'document',
                    //             title: '文档管理'
                    //         },
                    //         {
                    //             index: 'material',
                    //             title: '原材料管理'
                    //         }
                    //     ]
                    // },
                    // {
                    //     icon: 'el-icon-message',
                    //     index: 'tabs',
                    //     title: '消息通知'
                    // },
                    // {   
                    //     icon: 'el-icon-tickets',
                    //     index: '4',
                    //     title: '销售模块',
                    //     subs: [
                    //         {
                    //             index: 'marketunreview',
                    //             title: '未审核项目'
                    //         },
                    //         {
                    //             index: 'market',
                    //             title: '在建项目'
                    //         },
                    //         {
                    //             index: 'marketfinished',
                    //             title: '已完成项目'
                    //         },
                    //         {
                    //             index: 'marketcheck',
                    //             title: '订单管理'
                    //         }
                    //     ]
                    // }];
                });
            }
        }
    }
</script>

<style scoped>
    .sidebar{
        display: block;
        position: absolute;
        left: 0;
        height: 100%;
        top: 60px;
        bottom:0;
        overflow-y: scroll;  /*裁剪内容 - 提供滚动机制。 */
    }
    .sidebar::-webkit-scrollbar{
        width: 0;
    }
    .sidebar-el-menu:not(.el-menu--collapse){
        width: 150px;
    }
    .sidebar > ul {
        height:100%;
    }
    .user-avator{
        margin-top: 10px;
        margin-left: 12px;
    }
    .user-avator img{
        display: inline-block;
        width:40px;
        height:40px;
        border-radius: 50%;
    }
    .user-name{
        margin-left: 10px;
        display: inline-block;
        position: relative;
        top: -15px
    }
    .el-dropdown-link{
        color: #fff;
        cursor: pointer;
    }
    .btn-fullscreen{
        display: inline-block;
        transform: rotate(45deg);
        /* margin-right: 20px; */
        color: #fff;
    }
    .btn-bell, .btn-fullscreen{
        margin-left: 15px;
        font-size: 25px;
        display: inline-block;
        position: relative;
        width: 30px;
        height: 30px;
        text-align: center;
        border-radius: 15px;
        cursor: pointer;
    }
    .btn-bell-badge{
        position: absolute;
        right: 0;
        top: -2px;
        width: 8px;
        height: 8px;
        border-radius: 4px;
        background: #f56c6c;
        color: #fff;
    }
    .btn-bell .el-icon-bell{
        color: #fff;
    }
    .Other-btn{
        position: fixed;
        bottom: 0px;
    }
</style>
