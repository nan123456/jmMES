<template>
  <div>
    <!-- el-tabs的v-model对应el-tab-pane的name ,即显示对应标签页 -->
    <el-tabs type="border-card" v-model="activeName">
      <el-tab-pane name="first" class="main" label="制造BOM生成树">
        <el-button class="button_new" type="primary" @click="creatBOMTree()">新建BOM生成树</el-button><br/><br/>
        <div class="tittle">{{treename}}</div>
        <div v-if="showNULL">暂无制造BOM生成树</div>
        <!-- <div v-for="(item,index) in ListData" v-if="showList" :class="generateClassName(index)">
          <div class="row" style="line-height:40px">
              <div class="label">{{item.tree_name}}</div>
              <div class="btnGourd">
                <el-button type="primary" class="checkTreeBtn" @click="checkTree(item.list_id)">查看/修改树</el-button>
                <el-button type="danger" class="deleteTreeBtn" @click="deleteit(item.list_id,item.tree_name)">删除树</el-button>
                <el-button type="primary" class="jsonTreeBtn">生成json文件</el-button>
              </div>
          </div>
        </div> -->
        <el-table
          :data="ListData.filter(data => !search || data.name.toLowerCase().includes(search.toLowerCase()))"
          style="width: 100%">
          <el-table-column
            label="树列名称"
            width="200"
            prop="tree_name">
          </el-table-column>
          <el-table-column
            label="创建人"
            width="200"
            prop="user_name">
          </el-table-column>
          <el-table-column
            label="创建时间"
            width="200"
            prop="create_time">
          </el-table-column>
          <el-table-column
            align="right">
            <template slot-scope="scope">
              <el-button size="mini" type="primary" class="checkTreeBtn" @click="checkTree(scope.row.list_id)">查看/修改树</el-button>
                <el-button size="mini" type="danger" class="checkTreeBtn" @click="deleteit(scope.row.list_id,scope.row.tree_name)">删除树</el-button>
                <el-button size="mini" type="primary" class="checkTreeBtn" @click="creatJsonFile(scope.row.list_id)">生成json文件</el-button>
            </template>
            </el-table-column>
          </el-table>
        <PlmChangeTree class="PlmChangeTree" v-if="treeDisplay" @listen="listenChild" :treedata="treedata" :name="treename"></PlmChangeTree>
        <PlmRechange class="PlmChangeTree" v-if="RechangeTreeDisplay" @listen="listenRechangeChild" :treedata="rechangeTreedata" :list_id="list_id"></PlmRechange>
        <!-- 遮罩层
        <div class='popContainer' v-show="this.popContainershow"></div> -->
      </el-tab-pane>
      <el-tab-pane name="second" class="main" label="操作记录">
        <div class="tittle2">{{treename}}</div>
        <div v-if="showOprateNULL">暂无制造BOM树操作记录</div>
        <!-- <div v-for="(item,index) in OprateListData" v-if="showOprateList" :class="generateClassName(index)">
          <div class="row" style="line-height:40px">
              <div class="label">{{item.tree_name}}</div>
              <div class="btnGourd">
                <el-button type="primary" class="jsonTreeBtn" @click="checkOprate(item.tree_id)">查看操作记录</el-button>
              </div>
          </div>
        </div> -->
        <el-table
          :data="OprateListData.filter(data => !search || data.name.toLowerCase().includes(search.toLowerCase()))"
          style="width: 100%">
          <el-table-column
            label="树列名称"
            prop="tree_name">
          </el-table-column>
          <el-table-column
            label="创建人"
            prop="user_name">
          </el-table-column>
          <el-table-column
            label="创建时间"
            prop="create_time">
          </el-table-column>
          <el-table-column
            align="right">
            <template slot-scope="scope">
              <el-button
                size="mini"
                type="primary"
                @click="checkOprate(scope.row.tree_id)"
               >查看操作记录</el-button>
            </template>
            </el-table-column>
          </el-table>
        <PlmOprateData class="PlmChangeTree" v-if="OprateDataDisplay" @listen="listenOprateChild" :OprateListID="OprateListID"></PlmOprateData>
      </el-tab-pane>
    </el-tabs>
  </div>
</template>

<script>
import axios from 'axios'
import PlmChangeTree from './PLM_change_tree'
import PlmRechange from './PLM_rechange'
import PlmOprateData from './PLM_oprate_data'
export default {
  name: 'ProjectPart',
  components: {
      PlmChangeTree,
      PlmRechange,
      PlmOprateData
  },
  props: {
    name: String
  },
  created(){

  },
  data () {
    return {
      partdata:{},
      item:{},
      treedata:{},
      rechangeTreedata:{},
      list_id:'',
      activeName: 'first',
      treeDisplay:false,
      popContainershow:false,
      treename:'',
      showNULL:false,
      ListData:[],
      showList:false,
      RechangeTreeDisplay:false,
      showOprateNULL:false,
      showOprateList:false,
      OprateListData:[],
      OprateDataDisplay:false,
      OprateListID:'',
      operating_data:[],
      search:''
    }
  },// 监听数据的变化
  watch: {
    name : {
      immediate: true,   //如果不加这个属性，父组件第一次传进来的值监听不到
      handler(val) {
        this.treename=val;
        const that=this;
        var fd = new FormData()
        fd.append("flag","getPLMchangeTree")
        fd.append("product_id",val)
        axios.post(`${this.baseURL}/tree.php`,fd).then(function (res){
            that.treedata=res.data;
        })
        this.getTreeList(val)
        this.getOprateTreeList(val)

      },
    },
  },
  methods: {
    creatBOMTree(){
        this.treeDisplay=true
        this.popContainershow=true
        this.reloadTree()
    },
    listenChild(data){
        this.treeDisplay=data;
        // this.popContainershow=data;
        this.getTreeList(this.treename)
        this.getOprateTreeList(this.treename)  
    },
    listenRechangeChild(data){
        this.RechangeTreeDisplay=data;
        this.getOprateTreeList(this.treename)
    },
    listenOprateChild(data){
        this.OprateDataDisplay=data;
        this.getOprateTreeList(this.treename)  
    },
    reloadTree(){
        const that=this;
        var fd = new FormData()
        fd.append("flag","getPLMchangeTree")
        fd.append("product_id",this.treename)
        axios.post(`${this.baseURL}/tree.php`,fd).then(function (res){
            that.treedata=res.data;
        }) 
    },
    getTreeList(product_id){
        const that=this;
        var fd = new FormData()
        fd.append("flag","getTreeList")
        fd.append("product_id",product_id)
        axios.post(`${this.baseURL}/tree.php`,fd).then(function (res){
          if(res.data.success=='null'){
            that.showNULL=true;
            that.showList=false;
          }else if(res.data.success=='success'){
            // console.log(res.data.data)
            that.showNULL=false;
            that.showList=true;
            that.ListData=res.data.data;
          }
        })       
    },
    getOprateTreeList(product_id){
        const that=this;
        var fd = new FormData()
        fd.append("flag","getOprateTreeList")
        fd.append("product_id",product_id)
        axios.post(`${this.baseURL}/tree.php`,fd).then(function (res){
          if(res.data.success=='null'){
            that.showOprateNULL=true;
            that.showOprateList=false;
          }else if(res.data.success=='success'){
            // console.log(res.data.data)
            that.showOprateNULL=false;
            that.showOprateList=true;
            that.OprateListData=res.data.data;
          }
        })
    },
    checkTree(list_id){
        const that=this;
        this.list_id=list_id;
        var fd = new FormData()
        fd.append("flag","getRechangeTreeData")
        fd.append("list_id",list_id)
        axios.post(`${this.baseURL}/tree.php`,fd).then(function (res){
          console.log(res.data)
            that.rechangeTreedata=res.data;
        })       
        this.RechangeTreeDisplay=true;
    },
    generateClassName(index){
      if(index%2==0){
        return 'TreeList0';
      }else{
        return 'TreeList1';
      }
    },
    deleteit(id,name){
      this.$confirm('是否确认删除'+name, '提示', {
        confirmButtonText: '确定',
        cancelButtonText: '取消',
        }).then(() => {
            this.suredelete(id,name);             
        }).catch(() => {
            this.$message({
                type: 'info',
                message: '取消删除'
            });          
      });  
    },
    suredelete(id,name){
      const that=this;
      var fd = new FormData()
      fd.append("flag","deletePLMTree")
      fd.append("id",id)
      axios.post(`${this.baseURL}/tree.php`,fd).then(function (res){
        if(res.data.success=='success'){
            that.getTreeList(that.treename)
            that.insertDeleteData(id,name) 
            // that.$message({
            //     type: 'success',
            //     message: '删除成功'
            // });  
        }else{
            that.$message({
                type: 'error',
                message: '删除失败'
            }); 
        }
      })  
    },
    checkOprate(tree_id){ 
      this.OprateDataDisplay=true;
      this.OprateListID=tree_id;
    },
    insertDeleteData(id,name){
        var operating='删除了BOM树：'+name;
        this.operating_data.push(operating)
        var product_id=this.treename;
        const that=this;
        var fd = new FormData()
        fd.append("flag","savePlmOperatingData")
        fd.append("treename",name)
        fd.append("treeid",id)
        fd.append("product_id",product_id)
        fd.append("username",localStorage.username)
        fd.append("content",JSON.stringify(this.operating_data))
        axios.post(`${this.baseURL}/tree.php`,fd).then(function (res){
            if(res.data.success=='success'){
                that.operating_data=[];
                that.getOprateTreeList(that.treename)  
                that.$message({
                    type: 'success',
                    message: '删除成功'
                });   
            }
        }) 
    },
    creatJsonFile(listid){
        const that=this;
        var fd = new FormData()
        fd.append("flag","creatPlmJsonFile")
        fd.append("listid",listid)
        axios.post(`${this.baseURL}/tree.php`,fd).then(function (res){

        })      
    }
  }
}
</script>
<style scoped>
    .button_new{
        position: relative;
        float: right;
    }
    .checkTreeBtn{
        margin-left: 20px;
        height: 30px;

    }
    .deleteTreeBtn{
        margin-left: 10px;
        height: 30px;
    }
    .PlmChangeTree{
        position: absolute;
        z-index: 10;
        box-shadow:0 0 20px 5px gray
    }
    .TreeList0{
        z-index: 1;
        background-color: #f5f5f5;
        height: 40px;
    }
    .TreeList1{
        z-index: 1;
        height: 40px;      
    }
    .main{
      height: 400px;
      overflow: scroll;
    }
    .popContainer{
      position: fixed;
      top: 0;
      left: 0;
      right: 0;
      bottom: 0;
      background: rgba(0,0,0,0.5);
      z-index: 9;
    }
    .tittle{
      margin-top: -30px;
      font-size: 25px;
    }
    .tittle2{
      font-size: 25px;
    }
    .label{
      font-size:18px;
      float: left;
      width: 600px;
    }
    .jsonTreeBtn{
      margin-left: 10px;
      height: 30px;
    }
    .btnGourd{
      display: flex;
      flex-direction: row;
    }
    .row{
      display: flex;
      flex-direction: row;
      justify-content: space-between;
    }
</style>