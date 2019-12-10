<template>
  <div>
    <!-- <div class="crumbs">
      <el-breadcrumb separator="/">
        <el-breadcrumb-item><i class="el-icon-search"></i> 检验模块</el-breadcrumb-item>
      </el-breadcrumb>
    </div> -->
    <div class="examine">
      选择项目：
      <el-select v-model="selectvalue" placeholder="请选择">
        <el-option
          v-for="item in options"
          :key="item.value"
          :label="item.label"
          :value="item.value">
        </el-option>
      </el-select>      
      <el-tabs 
        v-model="activeName"
        @tab-click="handleClick"
      >
        <el-tab-pane 
          label="未检验" 
          name="first"
        ></el-tab-pane>
        <el-tab-pane 
          label="合格" 
          name="second"
        ></el-tab-pane>
        <el-tab-pane 
          label="不合格" 
          name="third"
        ></el-tab-pane>
        <examine-table :item="item" :selectvalue="selectvalue"></examine-table>
      </el-tabs>
    </div>
  </div>
</template>

<script>
import ExamineTable from './components/Table.vue'
import axios from "axios";
export default {
  name: "Examine",
  components: {
    ExamineTable
  },
  data () {
    return {
      activeName: 'first',
      item: '未检验',
      options: [],
      selectvalue:''
    }
  },
  created() {
    this.getProject();
  },
  methods: {
    handleClick (e) {
      this.item = e.label
    },
    //获取项目
    getProject(){
      // console.log(this.options)
      var fd = new FormData()
      fd.append('flag','getProject')
      axios.post(`${this.baseURL}/examine.php`,fd).then((res)=>{
        // console.log(res.data.data)
        this.options=res.data.data;
        this.selectvalue=res.data.data[0].value;
      })     
    }
  }
}
</script>

<style scoped>
  .iconfont {
    font-size: 14px;
  }
  .examine {
    padding: 30px;
    border-radius: 5px;
    background: #fff;
    min-height: 95vh;
  }
  
</style>