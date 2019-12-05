<template>
    <div class="main">
        <el-button type="danger" class="button_close" @click="close()">关闭</el-button>
        <div>
            <el-table
                class="table"
                :data="OprateListData"
                style="width: 100%"
                height="400px">
                <el-table-column
                    prop="product_id"
                    label="产品图号"
                    width="90">
                </el-table-column>
                <el-table-column
                    prop="tree_name"
                    label="树列名称"
                    width="110">
                </el-table-column>
                <el-table-column
                    prop="content"
                    label="更改内容">
                </el-table-column>
                <el-table-column
                    prop="create_user"
                    label="更改用户"
                    width="90">
                </el-table-column>
                <el-table-column
                    prop="create_time"
                    label="更改时间"
                    width="150">
                </el-table-column>
                <el-table-column
                    label="操作"
                    width="140">
                    <template slot-scope="scope">
                        <el-button @click="deleteSingle(scope.row.id)" type="primary" size="small">删除</el-button>
                    </template>
                </el-table-column>
            </el-table>  
        </div>       
    </div>
</template>
<script>
import axios from 'axios'
export default {
    data(){
        return{
            OprateListID:'',
            OprateListData:[]
        }
    },
    props: {
        OprateListID: String
    },
    watch: {
        OprateListID : {
            immediate: true,   //如果不加这个属性，父组件第一次传进来的值监听不到
            handler(val) {
                this.OprateListID=val;
                this.getList(val);
            }  
        },
    },
    methods:{
      close(){
          this.$emit('listen',false)
      },
      getList(OprateListID){
        const that=this;
        var fd = new FormData()
        fd.append("flag","getOprateListData")
        fd.append("OprateListID",OprateListID)
        axios.post(`${this.baseURL}/tree.php`,fd).then(function (res){
            if(res.data.success=='success'){
            that.OprateListData=res.data.data;
          }
        })
      },
      deleteSingle(id){
        this.$confirm('是否确认删除该条记录', '提示', {
        confirmButtonText: '确定',
        cancelButtonText: '取消',
        }).then(() => {
            this.sureDelete(id);
        }).catch(() => {
            this.$message({
                type: 'info',
                message: '已取消删除'
            });          
        });          
      },
      sureDelete(id){
        const that=this;
        var fd = new FormData()
        fd.append("flag","deletePLMOprateData")
        fd.append("id",id)
        axios.post(`${this.baseURL}/tree.php`,fd).then(function (res){
          if(res.data.success=='success'){
            that.getList(that.OprateListID);
            that.$message({
                type: 'success',
                message: '删除成功'
            });
          }
        }) 
      }
    }
}
</script>
<style scoped>
    .main{
        border-style: solid;
        border-width: 1px;
        border-color:rgba(0,0,0,0.5) ;
        width: 90%;
        position: absolute;
        top: 15px;
        left: 5%;
        height: 400px;
        background-color: white;
        
    }
    .button_close{
        position:absolute;
        right: 0.5%;
        top: 5px;
        z-index: 10;
    }
</style>