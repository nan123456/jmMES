<template>
    <div>
        <div class="searchWord">
                <el-input v-model="search" style="display: inline-block;width: 85%" 
                    placeholder="请输入搜索内容">
                </el-input>
                <el-button type="primary" @click="getselectData()">搜索</el-button>
                <!-- <el-button type="danger" @click="getData('Undelivered'),search='',pageSize=10">重置</el-button> -->
                <el-button type="danger" @click="reload1">重置</el-button>
            </div>       
        <el-table 
        :data="tableData.slice( (currentPage-1)*pageSize, currentPage*pageSize)" 
        style="width: 100%"        
        stripe 
        >
            <!-- <el-table-column
                type="selection"
                width="55"
                reserve-selection
            >
            </el-table-column> -->
            <el-table-column
                prop="modid"
                width="60"
                label="modid"
                v-if=false
            >
            </el-table-column>
            <el-table-column
                prop="partid"
                width="60"
                label="partid"
                v-if=false
            >
            </el-table-column>
            <el-table-column
                prop="fid"
                width="60"
                label="fid"
                v-if=false
            >
            </el-table-column>
            <el-table-column
                prop="routeid"
                width="60"
                label="routeid"
                v-if=false
            >
            </el-table-column>
            <el-table-column
                prop="product_name"
                label="项目名称"
                :filters="product_name"
                :filter-method="filterHandler"
                column-key="product_name"
            >
            </el-table-column>
            <el-table-column
                prop="number"
                label="工单号"
                :filters="pNumber"
                :filter-method="filterHandler"
                column-key="pNumber"
            >
            </el-table-column>
            <el-table-column
                prop="figure_number"
                label="零件图号"
                sortable
            >
            </el-table-column>
            <el-table-column
                prop="name"
                label="零件名称"
                sortable
            >
            </el-table-column>
            <el-table-column
                prop="child_material"
                label="规格"
                width="180"
                :filters="fChild_material"
                :filter-method="filterHandler"
                column-key="child_material"
            >
            </el-table-column>
            <el-table-column
                prop="route"
                label="加工工艺路线"
                :filters="RouteClass"
                :filter-method="filterHandler"
                column-key="RouteClass"
            >
            </el-table-column>
            <el-table-column
                prop="count"
                label="数量"
                sortable
            >
            </el-table-column>
            <el-table-column
                prop="stime"
                label="就工时间"
                sortable
            >
            </el-table-column>
            <el-table-column
                prop="ftime"
                label="完工时间"
                sortable
            >
            </el-table-column>
            <el-table-column
                prop="workinghours"
                label="工时"
                sortable
            >
            </el-table-column>
        </el-table>
                <!-- 数据分页 -->
        <div class="block">
          <el-pagination
            @size-change="handleSizeChange"
            @current-change="handleCurrentChange"
            :current-page="currentPage"
            :page-sizes="[10, 25, 50, 100]"
            :page-size="pageSize"
            layout="total, sizes, prev, pager, next, jumper"
            :total="tableData.length">
          </el-pagination>
        </div>
    </div>
</template>

<script>
import { VueGoodTable } from "vue-good-table";
import axios from "axios";
export default {
    name: "TableList",
    data(){
        return {
            currentPage: 1,
            pageSize: 10,
            fChild_material: [],
            tableData: [],
            FChild_material: [],
            search: '',
            product_name:[],
            pNumber:[],
            RouteClass:[
                {text:"K",value:"K"},
                {text:"K坡",value:"K坡"},
                {text:"S安装补贴",value:"S安装补贴"},
                {text:"S玻璃钢",value:"S玻璃钢"},
                {text:"S厂检",value:"S厂检"},
                {text:"S电气",value:"S电气"},
                {text:"S调试",value:"S调试"},
                {text:"S钢结构",value:"S钢结构"},
                {text:"S国（省）检",value:"S国（省）检"},
                {text:"S派人维修",value:"S派人维修"},
                {text:"S移交客户",value:"S移交客户"},
                {text:"S座舱",value:"S座舱"},
                {text:"F成型",value:"F成型"},
                {text:"F翻模",value:"F翻模"},
                {text:"F模具",value:"F模具"},
                {text:"F喷涂",value:"F喷涂"},
                {text:"F装配",value:"F装配"},
                {text:"M木工",value:"M木工"},
                {text:"GS",value:"GS"},
                {text:"G接线",value:"G接线"},
                {text:"G装灯",value:"G装灯"},
                {text:"G装箱",value:"G装箱"},
                {text:"T粗",value:"T粗"},
                {text:"T淬",value:"T淬"},
                {text:"T调",value:"T调"},
                {text:"T发黑",value:"T发黑"},
                {text:"T焊",value:"T焊"},
                {text:"T划线",value:"T划线"},
                {text:"T坡",value:"T坡"},
                {text:"T退",value:"T退"},
                {text:"T线",value:"T线"},
                {text:"T正火",value:"T正火"},
                {text:"T装",value:"T装"},
                {text:"TK",value:"TK"},
                {text:"IA",value:"IA"},
                {text:"IA1",value:"IA1"},
                {text:"IB",value:"IB"},        
                {text:"ID",value:"ID"},  
                {text:"IG",value:"IG"},  
                {text:"IS",value:"IS"},  
                {text:"I钻",value:"I钻"},  
                {text:"LK",value:"LK"},  
                {text:"L焊",value:"L焊"}, 
                {text:"L转",value:"L转"},  
                {text:"L装",value:"L装"},  
                {text:"J探",value:"J探"},   

            ],
        }
    },
    components: {
        VueGoodTable
    },
    mounted() {
        var flag = "Undelivered"
        this.getData(flag)
    },
    methods:{
        //重置按钮
        reload1(){
            this.search = '';
            var flag = "Undelivered";
            this.getData(flag);
        },
        //获取数据
        getData(flag) {
            var flag = flag
            var fd = new FormData()
                fd.append("flag",flag)
            axios
                .post(`${this.baseURL}/basecharts/list.php`,fd)
                .then(this.getDataSucc);
         },
         getDataSucc(res) {
             this.ResetBox();
             if(res.data.success==true){
                 //  console.log(res.data)
                res = res.data
                if (res.rows) {
                    this.fChild_material = [],
                    // 未排产
                    this.tableData = res.rows
                    if(res.fChild_material){
                    let columns = this.columns
                    let data_length = res.rows.length
                    // checkbox 规格赋值
                    let length2 = res.fChild_material.length
                    for(let i=0; i < length2; i++) {
                        this.fChild_material.push({text:res.fChild_material[i].f5,value:res.fChild_material[i].f5})
                    }
                    let length3 = res.product_name.length
                    for(let i=0; i < length3; i++) {
                        this.product_name.push({text:res.product_name[i].f6,value:res.product_name[i].f6})
                    }
                    let length4 = res.pNumber.length
                    for(let i=0; i < length4; i++) {
                        this.pNumber.push({text:res.pNumber[i].f7,value:res.pNumber[i].f7})
                    }
                }  
                } 
             }else{
                 alert("暂无数据") 
            }   
         },        
        filterHandler(value, row, column) {
        const property = column["property"];
        /* 
            this.pageSize设置值是因为element table里只对本页数据进行过滤
            因此设置此值相当于一页进行多少条数据进行过滤
        */
        this.pageSize = 1000000;
        return row[property] === value;
        },
        // 设置table每页显示条数
        handleSizeChange(val) {
        this.pageSize = val;
        this.currentPage = 1;
        },
        // 设置当前页数
        handleCurrentChange(val) {
        this.currentPage = val;
        },
        //重置筛选下拉框
        ResetBox(){
            this.fChild_material = [];
            this.product_name = [];
            this.pNumber = [];
        },        
        //获取模糊查询数据
        getselectData() {
            var fd = new FormData()
                fd.append("flag",'selectData')
                fd.append("select",this.search)
            axios
                .post(`${this.baseURL}/basecharts/list.php`,fd)
                .then(this.getDataSucc);
         },        
    },
}
</script>
<style>
.el-table-filter {
    max-height: 700px !important; 
    overflow: auto ;
}
</style>