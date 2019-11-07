<template>
  <div class="table">
        <!-- <div class="crumbs">
            <el-breadcrumb separator="/">
                <el-breadcrumb-item><i class="el-icon-tickets"></i> 已完成项目</el-breadcrumb-item>
            </el-breadcrumb>
        </div> -->
        <el-tabs v-model="tabName" @tab-click="handleClick">
          <!-- <el-tab-pane label="普通零部件" name="ordinary"></el-tab-pane> -->
          <el-tab-pane label="关键零部件" name="momentous"></el-tab-pane>
          <!-- <el-tab-pane label="全部部件" name="all"></el-tab-pane>
          <el-tab-pane label="外部协助" name="exterior"></el-tab-pane> -->
        </el-tabs>
        <div class="container">
          <el-container style="height: 600px;">
            <el-container style="height: 600px;">
              <el-aside width="300px">
                  <el-form :inline="true">
                    <el-form-item v-show="inputshow"> 
                        <el-select v-model="selectvalue" class="selectdiv" placeholder="请选择需要查询的项目">
                          <el-option
                            v-for="item in options"
                            :key="item.value"
                            :label="item.label"
                            :value="item.value">
                          </el-option>
                        </el-select>
                      <el-input 
                        placeholder="输入部件名称"
                        v-model="filterText"
                        style="width:130px">
                      </el-input>
                      <el-button type="primary" @click="handleFifter()" class="tree_btn" style="margin-left:5px">查询</el-button>
                      <el-button @click="resolve()" class="tree_btn" style="margin-righr:-2px">重置</el-button>
                    </el-form-item>
                  </el-form>
                  <!-- tree控件 -->
                  <el-tree
                    v-if="updateTree1"
                    class="filter-tree"
                    lazy
                    :load="loadNode"
                    :props="defaultProps"
                    @node-click="handleNodeClick"
                    node-key="id"
                    :default-expanded-keys="[0]"
                    :accordion="true"
                    :auto-expand-parent="false"
                    ref="tree">
                  </el-tree>

                  <!-- 搜索tree控件 -->
                  <el-tree
                    v-show="updateTree2"
                    class="filter-tree"
                    :data="result_arr"
                    :props="defaultProps"
                    @node-click="handleNodeClick">
                  </el-tree>
                </el-aside>
                <!-- 内容 -->
                <el-main>
                  <project v-if="this.lx=='xm'" :lxid="lxid"></project>
                  <part v-if="this.lx=='bj'" :lxid="lxid"></part>
                </el-main>
            </el-container>
          </el-container>
        </div>
         <el-dialog title="项目导入" :visible.sync="dialogUpload">
          <el-form >
            <el-form-item label="项目名称" :label-width="formLabelWidth">
              <el-input v-model="form.name" placeholder="请输入项目名称,如：过山车" auto-complete="off"></el-input>
            </el-form-item>
            <el-form-item label="项目编号" :label-width="formLabelWidth">
              <el-input v-model="form.number" placeholder="请输入项目编号，如:37#KSC-16A" auto-complete="off"></el-input>
            </el-form-item>
            <el-form-item label="项目类型" :label-width="formLabelWidth">
              <el-input v-model="form.type" placeholder="请输入项目类型，如：过山车" auto-complete="off"></el-input>
            </el-form-item>
            <el-form-item label="项目交付时间" :label-width="formLabelWidth">
              <!-- value-format 参数为设置date的类型 -->
              <el-date-picker type="date" value-format="yyyy-MM-dd"  placeholder="选择日期" v-model="form.date" ></el-date-picker>
            </el-form-item>
            <el-upload
              class="upload-demo"
              :before-upload="beforeupload"
              ref="upload"
              drag
              :limit="1"
              :data="form"
              :on-success="handleSuc"
              :action="uploadUrl"
              :auto-upload="false"
              style="margin-left:120px;">
              <i class="el-icon-upload"></i>
              <div class="el-upload__text">将文件拖到此处，或<em>点击上传</em></div>
              <div class="el-upload__tip" slot="tip">只能上传xls文件</div>
            </el-upload>
          </el-form>
        <div slot="footer" class="dialog-footer">
          <el-button @click="dialogUpload = false">取 消</el-button>
          <el-button type="primary" @click="uploadFile">保存</el-button>
        </div>
        </el-dialog>
    </div>
</template>

<script>
import axios from 'axios'
import Project from '../components/Project'
import Part from '../components/Part'
var key = '1'
export default {
  name: "CraftFinished",
  inject:["reload"],
  components:{
    Project,
    Part
  },
  data() {
      return {
        tabName:'ordinary',
        lx:'',
        lxid:'',
        uploadUrl:`${this.baseURL}/craft/project_upload.php`,
        filterText: '',
        updateTree1:true,
        updateTree2:false,
        dialogUpload:false,
        form:{},
        formLabelWidth: "120px",
        defaultProps: {
          children: 'children',
          label: 'name',
          isLeaf:'leaf'
        },
        quxiao:true,
        save:true,
        wait:false,
        inputshow:true,
        arr:[],
        result_arr:[],
        options: [],
        selectvalue:''
      };
    },
    mounted:function(){
      if(key=='1'){
        this.tabName = 'momentous'
      }else if(key=='5'){
        this.tabName = 'exterior'
      }else if(key=='6'){
        this.tabName = 'all'
      }
    },
  created() {
    this.getProject();
  },
    methods: {
       //头部标签页切换，2普通部件，1关键部件
      handleClick(tab,e){
        this.tabName=tab.name
        switch (tab.name){
          case 'momentous':
          key = 1
          this.updateTree1=true;
          this.updateTree2=false;
          break
          case 'all':
          key = 6
          this.updateTree1=true;
          this.updateTree2=false;
          break
          case 'exterior':
          key = 5
          this.updateTree1=true;
          this.updateTree2=false;
          break
        }
        this.reload()
      },
      // 过滤查询
      handleFifter() {
        if(this.selectvalue==''){
          alert("请选择要查询的项目")
        }else{
          this.updateTree1=false;
          this.updateTree2=true;
          var fd = new FormData()
          fd.append('flag','treefilter')
          fd.append('name',this.filterText)
          fd.append('pnumber',this.selectvalue)
          // fd.append('state',0)
          axios.post(`${this.baseURL}/tree.php`,fd).then((res)=>{
            // console.log(res.data.data[0])
            if(res.data.success == "success"){
              for(var i=0;i<res.data.data.length;i++){
                // console.log(i)
                // console.log(res.data.data[i])
                // console.log(res.data.data[i])
                this.arr[i]=res.data.data[i];   
              }  
              // console.log(this.nest(this.arr));
              this.result_arr=[];
              this.result_arr.push(this.nest(this.arr));
              // console.log(this.result_arr)
            }
          })
        }

      },
      //重制树
      resolve(){
        this.selectvalue='';
        this.filterText='';
        this.updateTree1=true;
        this.updateTree2=false;
      },
      //获取项目
      getProject(){
        // console.log(this.options)
        var fd = new FormData()
        fd.append('flag','getProject')
        axios.post(`${this.baseURL}/examine.php`,fd).then((res)=>{
          // console.log(res.data.data)
          this.options=res.data.data;
          // this.selectvalue=res.data.data[0].value;
        })     
      },
      //查询出来的一维数组转为嵌套数组
      nest(arrs){
        var result = arrs[0];
        var key ='children';
        var i=0;
        for(i=0;i<arrs.length-1;i++){
          arrs[i+1][key]=[result];
          result=arrs[i+1]
          // console.log(i)
          // console.log(result)
        }
          // console.log(result)
          return result;
      },
      // 文件上传成功时的钩子
      handleSuc(res,file, fileList) {
        // console.log(res)
        if(res.success == 'success'){
          alert("文件上传成功")
        }else  {
          alert("文件上传失败，文件格式错误或该项目已存在")
        }
        
      },
      // 上传文件前的钩子
      beforeupload (file){
        // console.log(file)
        this.form.ftype = file.type
      },
      // 上传文件
      uploadFile(){
        this.$refs.upload.submit()
      },
      // Tree点击事件
      handleNodeClick(data) {
        // console.log(data.id)
        // console.log(data.lx)
        this.lx = data.lx
        this.lxid = data.id 
        // console.log(this.$refs.tree.$children)
      },


      // Tree 控件显示
      loadNode(node, resolve){
          // 定义0级节点
          if(node.level === 0) {
            return resolve([{name:'大类',id:0,lx:'dl'}])   
            // console.log(node.data.id)     
          }
          // 大类节点
          if(node.level === 1&node.data.id === 0 ){
            // console.log(node.data.id)
            var fd = new FormData()
            fd.append("flag","finished_type")
            axios.post(`${this.baseURL}/tree.php`,fd).then(function (res){
              // console.log(res.data)
              if(res.data.success){
                return resolve (res.data.data)
              }else {
                return resolve([])
              }
            })  
          }
          // 项目节点
          if(node.level === 2)　{
            // console.log(node.data.name) 
            var fd = new FormData()
            fd.append('flag','finished_project')
            fd.append('type',node.data.name) //node.data 父节点所带参数
            axios.post(`${this.baseURL}/tree.php`,fd).then(function (res){
              // console.log(res)
              if(res.data.success){
                return resolve (res.data.data)
              }else {
                return resolve([])
              }
            })
          }
          // tree 3级树节点
          if(node.level === 3)　{
            // console.log(node.data.id) 
            var fd = new FormData()
            fd.append('flag','mpart')
            fd.append('key',key) //关键零部件判断
            fd.append('id',node.data.id) //node.data 父节点所带参数
            fd.append('name',node.data.zhname) //node.data 父节点所带参数
            fd.append('number',node.data.number) //node.data 父节点所带参数
            axios.post(`${this.baseURL}/tree.php`,fd).then(function (res){
              // console.log(res)
              if(res.data.success){
                return resolve (res.data.data)
              }else {
                return resolve([])
              }
            })
          }
          // 3级以下树子节点
          if(node.level > 3)　{
            console.log(node.data) 
            var fd = new FormData()
            fd.append('flag','part')
            fd.append('key',key) //关键零部件判断
            fd.append('pid',node.data.pid) //node.data 父节点所带参数
            fd.append('modid',node.data.modid) //node.data 父节点所带参数
            fd.append('name',node.data.name) //node.data 父节点所带参数
            fd.append('figure_number',node.data.figure_number) //node.data 父节点所带参数
            fd.append('level',node.level)  //判断当前节点处于何级
            axios.post(`${this.baseURL}/tree.php`,fd).then(function (res){
              // console.log(res)
              if(res.data.success){
                return resolve (res.data.data)
              }else {
                return resolve([])
              }
            })
          }
      }
    }
};
</script>
<style scoped>
  .el-header{
    height: 0px !important;
  }
  .filter-tree{
    overflow:auto;
    display: inline-block;
  }
  .tree_btn{
    width: 60px;
    text-align: center;
  }
  .selectdiv{
    margin-bottom: 10px;
    width: 275px
  }
</style>