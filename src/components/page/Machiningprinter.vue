<template>
    <div class="main" >
        <table class="machiningTableHeader">
                <tr>
                    <td colspan="8">产 品 机 械 加 工 工 艺 卡</td>
                    <td>编号</td>
                    <td colspan="4">{{this.machiningTableHeader.hendNumber}}</td>
                    <td></td>
                </tr>
                <tr>
                    <td>产品名称</td>
                    <td>{{this.machiningTableHeader.productName}}</td>
                    <td>所属部件名称</td>
                    <td>{{this.machiningTableHeader.ownPartName}}</td>
                    <td colspan="4">零件名称</td>
                    <td>{{this.machiningTableHeader.partsName}}</td>
                    <td colspan="4">材料</td>
                    <td>{{this.machiningTableHeader.workpieceNumber}}</td>
                </tr>
                <tr>
                    <td>产品图号</td>
                    <td>{{this.machiningTableHeader.productDrawingNumber}}</td>
                    <td>所属部件图号</td>
                    <td>{{this.machiningTableHeader.ownPartDrawingNumber}}</td>
                    <td colspan="4">零件图号</td>
                    <td>{{this.machiningTableHeader.partsDrawingNumber}}</td>
                    <td colspan="4">数量</td>
                    <td>{{this.machiningTableHeader.quantity}}</td>
                </tr>
            </table>
            <table class="machiningTableBody">
                <tr>
                <td class="serialNumber">序号</td>
                <td>工序</td>
                <td>车间</td>
                <td>工序内容</td>
                <td>设备</td>
                <td>工艺示意图</td>
                </tr>
                <tr>
                <tr  v-for="(item,index) in machiningTableBody.rowsData" :key="index">
                <td class="serialNumber">{{item.serialNumber}}</td>
                <td >{{item.processFlow}}</td>
                <td >{{item.workshop}}</td>
                <td >{{item.skillsRequirement}}</td>
                <td >{{item.equipment}}</td>
                <td rowspan="20" v-if="index==0"></td>
                </tr>  
                <tr>
                    <td colspan="5" class="machiningTableBody_1_img" @drop="drop($event,'1')" @dragover="allowDrop($event)" v-html="machiningTableBody.imgHtmlOne">&nbsp;</td>
                </tr>
            </table>
            <table class="machiningTableFooter">
                <tr>s
                <td></td>
                <td></td>
                <td></td>
                <td></td>
                <td colspan="2">编制（日期）</td>
                <td colspan="2">审核（日期）</td>
                <td colspan="5" rowspan="3">中山市金马科技娱乐设备股份有限公司</td>
                </tr>
                <tr>
                <td>&nbsp;</td>
                <td></td>
                <td></td>
                <td></td>
                <td></td>
                <td colspan="2" rowspan="2">{{this.machiningTableFooter.name1}}</td>
                <td colspan="2" rowspan="2">{{this.machiningTableFooter.name2}}</td>
                </tr>
                <tr>
                <td>标记</td>
                <td>处数</td>
                <td>更改文件号</td>
                <td>签名</td>
                <td>日期</td>
                </tr>
            </table>
        
	</div>
</template>

<script>
import axios from 'axios'
export default {
    name: 'MachiningprinterAll',
    data (){
        return{
            selectedTreeNode : {//左边树选中的节点（表类型，对应的id）
                tableFlag : "",
                relateId : ""
            },
            MachiningVisible: false,//机加工dialog
            machiningTableHeader: {
                "contactId" : "",//保存记录后返回的id
                "hendNumber": "",
                "productName": "",
                "ownPartName": "",
                "partsName": "",
                "workpieceNumber": "",
                "productDrawingNumber": "",
                "ownPartDrawingNumber": "",
                "partsDrawingNumber": "",
                "quantity": "",
                "pnumber":""//工单号
            },
            machiningTableBody: {
                rowsData:[
                {"serialNumber":"","processFlow":"","workshop":"","skillsRequirement":"","equipment":""},
                {"serialNumber":"","processFlow":"","workshop":"","skillsRequirement":"","equipment":""},
                {"serialNumber":"","processFlow":"","workshop":"","skillsRequirement":"","equipment":""},
                {"serialNumber":"","processFlow":"","workshop":"","skillsRequirement":"","equipment":""},
                {"serialNumber":"","processFlow":"","workshop":"","skillsRequirement":"","equipment":""},
                {"serialNumber":"","processFlow":"","workshop":"","skillsRequirement":"","equipment":""},
                {"serialNumber":"","processFlow":"","workshop":"","skillsRequirement":"","equipment":""},
                {"serialNumber":"","processFlow":"","workshop":"","skillsRequirement":"","equipment":""}
                ],
                fileOne:"",
                imgHtmlOne:""
            },   
            machiningTableFooter: {
                "name1":"",
                "name2":""
            }
        
        }
    },
    created(){
        this.getInfoData()
    },
    methods:{
         //根据id获取数据
        getInfoData() {
            //清除默认样式
            document.getElementsByTagName("html")[0].style.overflow = "visible"
            document.getElementsByTagName("body")[0].style.overflow = "visible"
            document.getElementById("app").style.overflow = "visible"
            
            let contactId = this.$route.query.contactId           
            axios.get(`${this.baseURL}/basicdata/maching.php?flag=getMachiningInfoData&contactID=${contactId}`)
            .then((response) => {  
                console.log(response);
                      
                if(response.data.state == "success"){
                    this.machiningTableHeader = response.data.data.machiningTableHeader
                    this.machiningTableFooter = response.data.data.machiningTableFooter    
                    this.machiningTableBody = response.data.data.machiningTableBody          
                                            
                    //显示图片
                    if(this.machiningTableBody.fileOne){//图片一
                        let imgsrc = `${this.baseURL}`+'/'+this.machiningTableBody.fileOne
                        this.machiningTableBody.imgHtmlOne = '<img src="'+imgsrc+'" style="max-width:100%;max-height:100%;pointer-events:none;">'
                    }  
                }
            })
            .catch(function(error){
                console.log(error+"axios error!")
            })
        },
        //返回材质及规格
        returnValueMaterialAndSpecifications(v1,v2){
            if(v1 && v2){
                return v1+"/t"+v2
            }else{
                return ''
            }            
        },
        //返回焊材及规格
        returnValueConsumablesAndSpecifications(v1,v2){              
            if(v1 && v2){
                return v1+"-"+v2
            }else{
                return ''
            }
        },
        //焊接层次返回空
        returnNull(v){
            if(v > 0){
                return v
            }else{
                return ''
            }
        },
        //判断是否有值
        isEmpty(v){
            if(v.length > 0){
                return 'block'
            }else{
                return 'none'
            }
        }
    }

}
</script>

<style scoped>
    .main {
        width: 100%;
        align-content: center;
    }
    table,tr {
        border-collapse: collapse;
        text-align: center;
    }
    table {
        border-collapse: collapse;
    }
    td {
        border: 1px solid black;
        border-collapse: collapse;
        padding-left: 1mm;
        padding-right: 1mm;
    }

    /* 表格头部 */
    .tableHeader {
        width: 277mm;
        text-align: center;
    }
    /* 表格尾部 */
    .tableFooter {
        width: 277mm;
        text-align: center;
    }
    /* 模板一 */
    /* .tableFirstModel {
        width: 277mm;
        text-align: center;
        font-size: 10.5pt;
    }
    .craftsmanshipTableBody_1_img{
        width: 100%;
        height: 280px;
    } */
    /* 模板二 */
    /* .tableSecondModel {
        width: 277mm;
        text-align: center;
        font-size: 10.5pt;
    }
    .craftsmanshipTableBody_2_img{
        width: 33%;
        height: 300px;
    }
    /* 模板三 */
    /* .tableThirdModel {
        width: 277mm;
        height: 550px;
        text-align: center;
    } */
    /* .tableThirdModel td {
        width: 100%;
        height: 100%;
        text-align: center;
     */

    .page {
        page-break-before: always;
    }
    .A4page {
        width: 277mm;
        height: 190mm;       
    }
    .mintabfont {
        font-size: 7pt;
    }
    /* 头部的表的信息 */
    .tabheader {
        width: 277mm;
        text-align: center;
    }
    /*尾部信息*/
    .tabfooter {
        width: 277mm;
        text-align: center;
    }
    .machiningTableFooter{
        width: 277mm;
        text-align: center;
        font-size: 23px;
    }
    .machiningTableBody{
        width: 277mm;
        text-align: center;
    }
    .machiningTableHeader{
        width: 277mm;
        text-align: center;
    }
    .machiningTableBody_1_img{
        width: 100%;
        border-collapse: collapse;
        text-align: center;
        height: 350px;
        width: 277mm;
  }
    /*第一页的第一个表格*/
    /* .pageOneFirst {
        width: 277mm;
        text-align: center;
    } */
    /*第一页的第二个表格*/
    /* .pageOnetabSecond {
        width: 277mm;
        text-align: center;
    }
    .pageOnetabSecond td {
        height: 7mm;
    } */


    /*第二页的主体内容*/
    /* .tabbodytwo {
        width: 277mm;
        text-align: center;
        font-size: 10.5pt;
    }
    .tabbodytwo td {
        height: 7mm;
    }
    .tabbodytwofont {
        font-size: 9pt;
    } */

    /*第三页的主体内容*/
    /* .tabbodythree {
        width: 277mm;
        text-align: center;
        
    } */

    /*小三*/
    .bigfontsize {
        font-size: 16pt;
    }
    /*小四*/
    .midfontsize {
        font-size: 11.2pt;
    }
    /*小六*/
    .minfontsize {
        font-size: 5.3pt;
    }

</style>

