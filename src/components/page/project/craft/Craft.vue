<template>
  <div class="table">
        <!-- <div class="crumbs">
            <el-breadcrumb separator="/">
                <el-breadcrumb-item><i class="el-icon-tickets"></i> 在建项目</el-breadcrumb-item>
            </el-breadcrumb>
        </div> -->
        <el-tabs v-model="tabName" @tab-click="handleClick">
          <!-- <el-tab-pane label="普通零部件" name="ordinary"></el-tab-pane> -->
          <el-tab-pane label="关键零部件" name="momentous"></el-tab-pane>
          <el-tab-pane label="进行中" name="ongoing"></el-tab-pane>
          <el-tab-pane label="已完成" name="completed"></el-tab-pane>
          <el-tab-pane label="外部协助" name="exterior"></el-tab-pane>
          <el-tab-pane label="全部部件" name="all"></el-tab-pane>
          <el-tab-pane label="PLM获取部件" name="plm"></el-tab-pane>
        </el-tabs>
        <div class="container">
          <el-container style="height: 600px;">
            <el-header>
              <el-row :gutter="20">
                <el-col :span="2" :offset="20">
                  <el-button type="primary" icon="el-icon-upload" @click="dialogUpload = true">导入</el-button>
                </el-col>
              </el-row>
            </el-header>
            <el-container style="height: 600px;">
              <el-aside width="250px">
                  <el-form :inline="true">
                    <el-form-item>
                      <el-input 
                        placeholder="输入modID"
                        v-model="filterText"
                        style="width:150px">
                      </el-input>
                      <el-button type="primary" @click="handleFifter()">查询</el-button>
                      </el-form-item>
                  </el-form>
                  <!-- tree控件 -->
                  <el-tree
                    v-if="updateTree"
                    class="filter-tree"
                    lazy
                    :load="loadNode"
                    :props="defaultProps"
                    @node-click="handleNodeClick"
                    :accordion="true"
                    :auto-expand-parent="false"
                    ref="tree">
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
            <el-form-item label="工单号" :label-width="formLabelWidth">
              <el-input v-model="form.pnumber" placeholder="请输入工单号，如：37#" auto-complete="off"></el-input>
            </el-form-item>
            <el-form-item label="项目名称" :label-width="formLabelWidth">
              <el-input v-model="form.name" placeholder="请输入项目名称，如：中山过山车" auto-complete="off"></el-input>
            </el-form-item>
            <el-form-item label="项目图号" :label-width="formLabelWidth">
              <el-input v-model="form.number" placeholder="请输入项目图号，如:KSC-16A" auto-complete="off"></el-input>
            </el-form-item>
            <el-form-item label="项目类型" :label-width="formLabelWidth">
              <!-- <el-input v-model="form.type" placeholder="请输入项目类型，如：过山车" auto-complete="off"></el-input> -->
              <el-select v-model="form.type" placeholder="请选择项目类型">
                <el-option label="转马类"  value="0_转马类"></el-option>
                <el-option label="滑行类" value="1_滑行类"></el-option>
                <el-option label="陀螺类" value="2_陀螺类"></el-option>
                <el-option label="飞行塔类" value="3_飞行塔类"></el-option>
                <el-option label="赛车类" value="4_赛车类"></el-option>
                <el-option label="自控飞机类" value="5_自控飞机类"></el-option>
                <el-option label="观览车类" value="6_观览车类"></el-option>
                <el-option label="小火车类" value="7_小火车类"></el-option>
                <el-option label="架空游览车类" value="8_架空游览车类"></el-option>
                <el-option label="水上游乐设施" value="9_水上游乐设施"></el-option>
                <el-option label="碰碰车类" value="10_碰碰车类"></el-option>
                <el-option label="电池车类" value="11_电池车类"></el-option>
                <el-option label="摇摆类" value="12_摇摆类"></el-option>
                <el-option label="回旋类" value="13_回旋类"></el-option>
                <el-option label="其他类" value="14_其他类"></el-option>
                <el-option label="科技娱乐类" value="15_科技娱乐类"></el-option>
              </el-select>
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
          <el-button @click="dialogUpload = false" v-if="quxiao">取 消</el-button>
          <el-button type="primary" @click="uploadFile" v-if="save">保存</el-button>
          <el-button v-if="wait">请稍等</el-button>
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
  name: "Craft",
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
        uploadUrl:`${this.baseURL}/importERP.php`,
        filterText: '',
        updateTree:true,
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
        wait:false
      };
    },
    mounted:function(){
      if(key=='1'){
        this.tabName = 'momentous'
      }else if(key=='3'){
        this.tabName = 'ongoing'
      }else if(key=='4'){
        this.tabName = 'completed'
      }else if(key=='5'){
        this.tabName = 'exterior'
      }else if(key=='6'){
        this.tabName = 'all'
      }else if(key=='7'){
        this.tabName = 'plm'
      }
    },
    methods: {
       //头部标签页切换，2普通部件，1关键部件,3进行中，4已完成
      handleClick(tab,e){
        this.tabName=tab.name
        switch (tab.name){
          case 'completed':
          key = 4
          break
          case 'ongoing':
          key = 3
          break
          case 'ordinary':
          key = 2
          break
          case 'momentous':
          key = 1
          break
          case 'exterior':
          key = 5
          break
          case 'all':
          key = 6
          break
          case 'plm':
          key = 7
          break
        }
        this.reload()
      },
      // 过滤查询
      handleFifter() {
        // console.log(this.filterText)
          this.updateTree = !this.updateTree
          // 增加延时确保tree组件重新渲染
          setTimeout(()=>{
            this.updateTree = !this.updateTree
          },500)
      },
      // 文件上传成功时的钩子
      handleSuc(res,file, fileList) {
        this.quxiao=true;
        this.wait=false;
        console.log(res)
        if(res.success == 'success'){
          alert("文件上传成功")
        }else  {
          alert("文件上传失败，文件格式错误或该项目已存在")
        }
        
      },
      // 上传文件前的钩子
      beforeupload (file){
        alert('时间可能会有点长，请稍等');
        this.quxiao=false;
        this.save=false;
        this.wait=true;
        // console.log(file)
        // this.form.ftype = file.type
        this.form.fname = file.name
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
        // 判断当前是否为查询状态
        // console.log(this.filterText)
        if(!this.filterText){
          // 定义0级节点
          if(node.level === 0) {
            return resolve([{name:'大类',id:0,lx:'dl'}])   
            // console.log(node.data.id)     
          }
          // 大类节点
          if(node.level === 1&node.data.id === 0 ){
            // console.log(node.data.id)
            var fd = new FormData()
            fd.append("flag","type")
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
            // console.log(node.data) 
            var fd = new FormData()
            fd.append('flag','project')
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
            console.log(node.data) 
            var fd = new FormData()
            fd.append('flag','mpart')
            fd.append('key',key) 
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
            fd.append('key',key) 
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
        } else {
          if(node.level === 0) {
            var fd = new FormData()
            fd.append('flag','treefilter')
            fd.append('modid',this.filterText)
            fd.append('state',0)
            axios.post(`${this.baseURL}/tree.php`,fd).then((res)=>{
              // console.log(res.data.data)
              if(res.data.success == "success"){
                return resolve(res.data.data)
              }else {
                return resolve([])
              }
            })
            // console.log(node.data.id)     
          }
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
</style>