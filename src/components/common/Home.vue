<template>
    <div class="wrapper">
        <v-head></v-head>
        <v-sidebar></v-sidebar>
        <div class="content-box main" :class="{'content-collapse':collapse}">
            <v-tags></v-tags>
            <div class="content">
                <transition name="move" mode="out-in">
                    <keep-alive :include="tagsList">
                        <router-view></router-view>
                    </keep-alive>
                </transition>
                <div class="globalFoot">
                    Copyright
                    <img class="globalFoot-img" src="../../assets/globalFoot.svg">
                    2019-2020 金马 版权所有<br>
                    <a href="http://www.beian.miit.gov.cn" target="_blank" rel="noopener noreferrer">粤ICP备14006151号-5</a>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
    import vHead from './Header.vue';
    import vSidebar from './Sidebar.vue';
    import vTags from './Tags.vue';
    import bus from './bus';
    export default {
        data(){
            return {
                tagsList: [],
                collapse: false
            }
        },
        components:{
            vHead, vSidebar, vTags
        },
        methods:{
            changeleft(){
                // document.getElementById("box").style.left="64px !important"
            },
        },
        created(){
            bus.$on('collapse', msg => {
                this.collapse = msg;
            })

            // 只有在标签页列表里的页面才使用keep-alive，即关闭标签之后就不保存到内存中了。
            bus.$on('tags', msg => {
                let arr = [];
                for(let i = 0, len = msg.length; i < len; i ++){
                    msg[i].name && arr.push(msg[i].name);
                }
                this.tagsList = arr;
            })
        }
    }
</script>
<style scoped>
.content {
padding: 10px !important;
}
.main{
    top: 0px;
}
.globalFoot{
    position: relative;
    bottom: 0px;
    text-align:center;
    font-size: 13px;
}
.globalFoot-img{
    width: 1.3em;
    height: 1.3em;
}
</style>