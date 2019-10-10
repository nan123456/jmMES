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
                label="产品名称"
                sortable
            >
            </el-table-column>
            <el-table-column
                prop="number"
                label="工单"
                sortable
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
                label="名称"
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
                sortable
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
            window.location.reload()
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
             if(res.data.success=="error"){
                 alert("暂无数据")
             }
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
            }  
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