<template>
    <div>
        <div style="width:10px;height:10px;background-color:#409EFF;float:left"></div>
        <p class="Pstyle">{{this.pdata[0].isfinish}}:{{this.pdata[0].count}}%</p>
        <div style="width:10px;height:10px;background-color:#67C23A;float:left"></div>
        <p class="Pstyle">{{this.pdata[1].isfinish}}:{{this.pdata[1].count}}%</p>
        <div style="width:10px;height:10px;background-color:#E6A23C;float:left"></div>
        <p class="Pstyle">{{this.pdata[2].isfinish}}:{{this.pdata[2].count}}%</p>
        <br>
        <div style="width:23.5%;height:20px;" v-html="DivHtml">
        </div>
    </div>
</template>
<script>
import axios from 'axios'
export default {
    data:function(){
        return{
            pdata:{"0":{"isfinish":"","count":0},"1":{"isfinish":"","count":0},"2":{"isfinish":"","count":0}},
            DivHtml:'',
            belong_part:'',
            pNumber:'',
            DivColor : ["#409EFF","#67C23A","#E6A23C","#F56C6C","#909399"]
        }
    },
    props: {
        ProgData:{
            type:Array
        }
    },
    watch:{
        ProgData : {
            immediate: true,   //如果不加这个属性，父组件第一次传进来的值监听不到
            handler(val) {
                // console.log(val)
                this. DivHtml = ''
                this.belong_part = val.name
                this.pNumber = val.pNumber
                this.getdata()
            }  
        }
    },
    // created(){
    //     this.getdata()
    // },
    methods:{
        getdata(){
            var fd = new FormData
            fd.append('flag',"Havebelong_part")
            fd.append('belong_part',this.belong_part)
            fd.append('pNumber',this.pNumber)
            axios.post(`${this.baseURL}/ProgressBar.php`,fd).then((res)=>{
                // console.log(res.data)
                this.LoadDiv(res.data)
          })
        },
        LoadDiv(data){
            this.pdata = data
            // console.log(this.pdata)
            for(var i=0;i<3;i++){
                this.DivHtml=this.DivHtml+`<div style='color: white;line-height: 20px;height: 20px;background-color:${this.DivColor[i]};float:left;width:${data[i].count}%;text-align:center'></div>`
            }
        }
    }
}
</script>
<style scoped>
.Pstyle{
    font-size: 2px;
    float: left;
}
</style>