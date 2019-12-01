<template>
  <div>
    <!-- el-tabs的v-model对应el-tab-pane的name ,即显示对应标签页 -->
    <el-tabs type="border-card" v-model="activeName">
      <el-tab-pane class="main" name="first" label="部件信息">
        <el-form ref="form"  label-width="100px">
          <div class="leftInput">
            <el-form-item label="项目图号">
              <el-col :span="10">
                <el-input class="input" v-model="partdata.product_id" readonly></el-input>
              </el-col>
            </el-form-item>
            <el-form-item label="名称">
              <el-col :span="10">
                <el-input class="input" v-model="partdata.label" readonly></el-input>
              </el-col>
            </el-form-item>
            <el-form-item label="零件图号">
              <el-col :span="10">
                <el-input class="input" v-model="partdata.figure_number" readonly></el-input>
              </el-col>
            </el-form-item>
            <el-form-item label="所属图号">
              <el-col :span="10">
                <el-input class="input" v-model="partdata.belong_part" readonly></el-input>
              </el-col>
            </el-form-item>
          </div>
          <div class="rightInput">
            <el-form-item label="层级">
              <el-col :span="10">
                <el-input class="input" v-model="partdata.hierarchy" readonly></el-input>
              </el-col>
            </el-form-item>
            <el-form-item label="材料">
              <el-col :span="10">
                <el-input class="input" v-model="partdata.material" readonly></el-input>
              </el-col>
            </el-form-item>
            <el-form-item label="数量">
              <el-col :span="10">
                <el-input class="input" v-model="partdata.count" readonly></el-input>
              </el-col>
            </el-form-item>
            <el-form-item label="备注">
              <el-col :span="10">
                <el-input class="input" v-model="partdata.remark" readonly></el-input>
              </el-col>
            </el-form-item>
          </div>
        </el-form>
      </el-tab-pane>
    </el-tabs>
  </div>
</template>

<script>
import axios from 'axios'
export default {
  name: 'ProjectPart',
  components: {

  },
  props: {
    lxid: String
  },
  data () {
    return {
      partdata:{},
      mylxid: '',
      item:{},
      activeName: 'first'
    }
  },// 监听数据的变化
  watch: {
    lxid : {
      immediate: true,   //如果不加这个属性，父组件第一次传进来的值监听不到
      handler(val) {
        // console.log(val)
        this.mylxid = val
      }  
    },
    // 声明mylxid的原因:vue不允许直接修改props的值，通过声明赋值新变量重新渲染组件
    mylxid: {
      immediate: true,   //如果不加这个属性，父组件第一次传进来的值监听不到
      handler(val) {
        console.log(val)
        var sch = new FormData() //定义获取partschdata的传值
        sch.append('id',val)
        sch.append('flag','plm_part')
        axios.post(`${this.baseURL}/part.php`,sch).then(this.getPartschdataSucc)
      }  
    }
  },
  methods: {
    getPartschdataSucc(res) {
      // console.log(res.data)
      this.partdata = {"item":[]}
      if(res.data.success) {
        this.partdata = res.data
      }else {
        this.partdata = {"key":"default"}
      }
    },
  }
}
</script>
<style scoped>
  .img{
    padding-left:7.5% ;
    height: 600px;
    width: 600px;
  }
  .leftInput{
    position: absolute;
    top: 15px;
    right: 650px;
  }
  .rightInput{
    position: absolute;
    top: 15px;
    right: 200px;
  }
  .main{
    height: 180px;
  }
  .input{
    width: 300px;
  }
</style>