<template>
  <div class="table">
        <!-- <div class="crumbs">
            <el-breadcrumb separator="/">
                <el-breadcrumb-item><i class="el-icon-tickets"></i> 在建项目</el-breadcrumb-item>
            </el-breadcrumb>
        </div> -->
        <el-tabs v-model="tabName" @tab-click="handleClick">
          <!-- <el-tab-pane label="普通零部件" name="ordinary"></el-tab-pane> -->
          <!-- <el-tab-pane label="关键零部件" name="momentous"></el-tab-pane> -->
          <!-- <el-tab-pane label="进行中" name="ongoing"></el-tab-pane> -->
          <!-- <el-tab-pane label="外部协助" name="exterior"></el-tab-pane> -->
          <el-tab-pane label="全部零部件" name="all"></el-tab-pane>
          <el-tab-pane label="已完成零部件" name="completed"></el-tab-pane>
          <el-tab-pane label="PLM数据图档读取" name="plm"></el-tab-pane> 
        </el-tabs>
        <div class="container">
          <el-container style="height: 600px;">
            <el-header>
              <el-row :gutter="20">
                <el-col :span="2" :offset="20">
                  <el-button type="primary" icon="el-icon-upload" @click="dialogUpload = true" v-if="(this.lx!=='xm'&&this.lx!=='bj'&&this.lx!=='plm_part'&&this.tabName !== 'plm')&&this.root_drzj">导入</el-button>
                </el-col>
              </el-row>
            </el-header>
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
                      <el-button @click="resolve()" class="tree_btn" style="margin-right:-2px">重置</el-button>
                    </el-form-item>
                    <el-form-item v-show="inputshow2"> 
                        <el-select v-model="selectvalue2" class="selectdiv2" placeholder="请选择需要读取的产品">
                          <el-option
                            v-for="item in options2"
                            :key="item.index"
                            :label="item.label"
                            :value="item.value">
                          </el-option>
                        </el-select>
                      <el-button type="primary" @click="getPLM()" class="tree_btn2" style="margin-right:-2px">读取设计文档</el-button>
                    </el-form-item>
                  </el-form>
                  <!-- tree控件 -->
                  <el-tree
                    v-show="updateTree1"
                    class="filter-tree"
                    lazy
                    :load="loadNode1"
                    :props="defaultProps"
                    @node-click="handleNodeClick"
                    node-key="id"
                    :default-expanded-keys="[0,1]"
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

                  <!-- plm获取树 -->
                  <el-tree
                    v-show="updateTree3"
                    class="filter-tree"
                    lazy
                    :load="loadNode2"
                    :props="defaultProps"
                    @node-click="handleNodeClick"
                    node-key="id"
                    :default-expanded-keys="[0]"
                    :accordion="true"
                    :auto-expand-parent="false"
                    ref="tree">
                  </el-tree>

                </el-aside>
                <!-- 内容 -->
                <el-main>
                  <el-button type="primary" class="ReturnButton" @click="CacheReturn()" v-show="btn_state">返回</el-button>
                  <project v-if="this.lx=='xm'" :lxid="lxid"></project>
                  <part @change="btn_state_change" v-if="this.lx=='bj'" :lxid="lxid"></part>
                  <plm-part v-if="this.lx=='plm_part'" :lxid="lxid"></plm-part>
                  <PlmTreeList v-if="this.lx=='plm_tree'" :name='name'></PlmTreeList>
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
          <el-button type="primary" @click="uploadFile" v-if="save">确认导入</el-button>
          <el-button v-if="wait">请稍等</el-button>
        </div>
        </el-dialog>
    </div>
</template>

<script>
import axios from 'axios'
import Project from '../components/Project'
import Part from '../components/Part'
import PlmPart from '../components/PLM_Part'
import PlmTreeList from '../components/PLM_tree_list'
var key = '6'
export default {
  name: "Craft",
  inject:["reload"],
  components:{
    Project,
    Part,
    PlmPart,
    PlmTreeList
  },
  data() {
      return {
        btn_state:false,
        seeModules:'',
        tabName:'ordinary',
        lx:'',
        lxid:'',
        uploadUrl:`${this.baseURL}/importERP.php`,
        filterText: '',
        updateTree1:true,
        updateTree2:false,
        updateTree3:false,
        dialogUpload:false,
        form:{},
        formLabelWidth: "120px",
        defaultProps: {
          children: 'children',
          label: 'name',
          isLeaf:'leaf'
        },
        root_drzj:false,
        quxiao:true,
        save:true,
        wait:false,
        inputshow:true,
        arr:[],
        result_arr:[],
        options: [],
        selectvalue:'',
        inputshow2:false,
        options2: [],
        selectvalue2:'',
        CacheLength:0,
        dataplmtree:'',
        name:''
      };
    },
  created() {
    this.getProject();
    this.getPLMProject();
    this.getroot_apply();
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
          key = 4;
          this.inputshow=true;
          this.inputshow2=false;
          this.updateTree1=true;
          this.updateTree2=false;
          this.updateTree3=false;
          this.reload()
          break
          case 'ongoing':
          key = 3;
          this.inputshow=true;
          this.inputshow2=false;
          this.updateTree1=true;
          this.updateTree2=false;
          this.updateTree3=false;
          this.reload()
          break
          case 'ordinary':
          key = 2;
          this.inputshow=true;
          this.inputshow2=false;
          this.updateTree1=true;
          this.updateTree2=false;
          this.updateTree3=false;
          this.reload()
          break
          case 'momentous':
          key = 1;
          this.inputshow=true;
          this.inputshow2=false;
          this.updateTree1=true;
          this.updateTree2=false;
          this.updateTree3=false;
          this.reload()
          break
          case 'exterior':
          key = 5;
          this.inputshow=true;
          this.inputshow2=false;
          this.updateTree1=true;
          this.updateTree2=false;
          this.updateTree3=false;
          this.reload()
          break
          case 'all':
          key = 6;
          this.inputshow=true;
          this.inputshow2=false;
          this.updateTree1=true;
          this.updateTree2=false;
          this.updateTree3=false;
          this.reload()
          break
          case 'plm':
          key = 7;
          this.inputshow=false;
          this.inputshow2=true;
          this.updateTree1=false;
          this.updateTree2=false;
          this.updateTree3=true;
          break
        }
        // this.reload()
      },
      // 过滤查询
      handleFifter() {
        if(this.selectvalue==''){
          alert("请选择要查询的项目")
        }else if(this.filterText==''){
          alert("请填写要查询的内容")
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
        this.lx='reload';
        this.selectvalue='';
        this.filterText='';
        this.updateTree1=true;
        this.updateTree2=false;
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
        // console.log(data)
        // console.log(data.lx)
        if(data.lx=='bj'){
          var CacheArray = [];
          CacheArray.push(data.id);
          sessionStorage.setItem('ReturnCache', JSON.stringify(CacheArray));
        }
        this.lx = data.lx
        this.lxid = data.id 
        this.name = data.name 
        // console.log(this.$refs.tree.$children)
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
      getPLMProject(){
        // console.log(this.options)
        var fd = new FormData()
        fd.append('flag','getPLMProject')
        axios.post(`${this.baseURL}/part.php`,fd).then((res)=>{
          // console.log(res.data.data)
          this.options2=res.data.data;
          // this.selectvalue=res.data.data[0].value;
        }) 
      },
      getPLM(){
          // alert("未检测到PLM数据源，请在同一局域网下读取");
          this.dataplmtree=this.loadNode2();
          console.log(this.dataplmtree)
      },
      // Tree 控件显示
      loadNode1(node, resolve){
          // 定义0级节点
          if(node.level === 0) {
            return resolve([{name:'产品大类',id:0,lx:'dl'}])   
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
            // console.log(node.data) 
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
            // console.log(node.data) 
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
      },


      loadNode2(node, resolve){
          // 定义0级节点
          if(node.level === 0) {
            return resolve([{name:'产品图号',id:0,lx:'dl'}])    
          }
          // 大类节点
          if(node.level === 1&node.data.id === 0 ){
            // console.log(node.data.id)
            var fd = new FormData()
            fd.append("flag","plm_type")
            axios.post(`${this.baseURL}/tree.php`,fd).then(function (res){
              // console.log(res.data)
              if(res.data.success){
                return resolve (res.data.data)
              }else {
                return resolve([])
              }
            })  
          }
          // // 项目节点
          // if(node.level === 2)　{
          //   console.log(node.data) 
          //   var fd = new FormData()
          //   fd.append('flag','plm_project')
          //   fd.append('type',node.data.name) //node.data 父节点所带参数
          //   axios.post(`${this.baseURL}/tree.php`,fd).then(function (res){
          //     // console.log(res)
          //     if(res.data.success){
          //       return resolve (res.data.data)
          //     }else {
          //       return resolve([])
          //     }
          //   })
          // }
          // tree 2级树节点
          if(node.level === 2)　{
            console.log(node.data) 
            var fd = new FormData()
            fd.append('flag','plm_mpart')
            fd.append('number',node.data.name) //node.data 父节点所带参数
            axios.post(`${this.baseURL}/tree.php`,fd).then(function (res){
              // console.log(res)
              if(res.data.success){
                return resolve (res.data.data)
              }else {
                return resolve([])
              }
            })
          }
          // 2级以下树子节点
          if(node.level > 2)　{
            console.log(node.data) 
            var fd = new FormData()
            fd.append('flag','plm_part')
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
      },
      //通过缓存进行返回
      CacheReturn(){
        var CacheArray = JSON.parse(sessionStorage.getItem('ReturnCache'));
        this.lxid = '';
        var new_lxid= CacheArray[CacheArray.length-2];
        this.$nextTick(() => (this.lxid =new_lxid))
        // this.lxid = CacheArray[CacheArray.length-2];
        // console.log(this.lxid);
        CacheArray.pop();
        sessionStorage.setItem('ReturnCache', JSON.stringify(CacheArray));
        if(CacheArray.length>1){
          this.btn_state=true
        }else{
          this.btn_state=false
        }
      },
      btn_state_change(btn_state){
        // console.log(btn_state)
        this.btn_state=btn_state

      },
      //通过缓存用户account与传参cell获取权限，返回一个布尔值
      //通过 async await进行同步处理axios
      async getroot(cell){
        let ms_username=localStorage.ms_username;
        let fd = new FormData();
        //在axios中用that指向
        let that=this;
        var arr=[];
        fd.append('flag','getSeeModules');
        fd.append('account',ms_username);
        var axios_res =  await axios.post(`${this.baseURL}/getSeeModules.php`,fd).then(function (res){
          arr=res.data.data.split(",");
        })
        //返回一个结果布尔值
         return arr.includes(cell)
      },
      //通过getroot函数获取权限
      getroot_apply(){
        let that=this;
        //用了async异步，内部访问return
        this.getroot("14_DRZJ").then(function(res){
          that.root_drzj=res;
        });
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
  .selectdiv2{
    margin-bottom: 10px;
    width: 180px   
  }
  .tree_btn2{
    width: 100px;
    text-align: center;   
  }
  .ReturnButton{
    z-index: 5;
    position: relative;
    top:0px;
    float: right;
    margin: 5px
  }
</style>