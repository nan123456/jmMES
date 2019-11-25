<template>
  <div>
    <!-- el-tabs的v-model对应el-tab-pane的name ,即显示对应标签页 -->
    <el-tabs type="border-card" v-model="activeName">
      <el-tab-pane name="first" label="制造BOM生成树">
        <!-- <el-button class="button_new" type="primary" @click="creatBOMTree()">新建BOM生成树</el-button><br/><br/> -->
        <el-button class="button_new" type="danger" @click="reloadTree">重载树</el-button>
        <!-- <div>暂无制造BOM生成树</div> -->
        <!-- <div class="TreeList">
            <label style="font-size:18px">树名称</label>
            <el-button type="primary" class="checkTreeBtn">查看树</el-button>
            <el-button type="danger" class="deleteTreeBtn">删除树</el-button>
        </div> -->
        <PlmChangeTree class="PlmChangeTree" v-if="treeDisplay" @listen="listenChild" :treedata="treedata"></PlmChangeTree>
      </el-tab-pane>
    </el-tabs>
  </div>
</template>

<script>
import axios from 'axios'
import PlmChangeTree from './PLM_change_tree'
export default {
  name: 'ProjectPart',
  components: {
      PlmChangeTree
  },
  props: {
    name: String
  },
  data () {
    return {
      partdata:{},
      item:{},
      treedata:{},
      activeName: 'first',
      treeDisplay:true
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
      }  
    },
  },
  methods: {
    creatBOMTree(){
        this.treeDisplay=true

    },
    listenChild(data){
        this.treeDisplay=data;
    },
    reloadTree(){
        const that=this;
        var fd = new FormData()
        fd.append("flag","getPLMchangeTree")
        fd.append("product_id",this.treename)
        axios.post(`${this.baseURL}/tree.php`,fd).then(function (res){
            that.treedata=res.data;
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
        margin-left: 20px
    }
    .deleteTreeBtn{
        margin-left: 10px
    }
    .PlmChangeTree{
        position:absolute;
        top: 0;
        left: 0;
        z-index: 20;
    }
    .TreeList{
        position:relative;
        z-index: 1;
    }
</style>