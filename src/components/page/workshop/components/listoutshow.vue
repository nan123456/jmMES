<template>
  <div>
    <vue-good-table
      :columns="columns"
      :rows="rows"
      @on-column-filter="selectionChanged"
      :search-options="{enabled: true}"
      :pagination-options="{enabled: true,mode: 'records'}"
    >
      <template slot="table-row" slot-scope="props">
        <span v-if="props.column.field == 'operate'">
          <el-button type="primary"   @click="listQRcode(props.row)">二维码</el-button>
          <!-- <el-button type="primary"   @click="equipmentBarcode(props.row)">条形码</el-button> -->
          <el-button type="primary"   @click="handleTabledata(props.row)">查看详情</el-button>
          <el-button type="primary" icon="el-icon-edit" circle @click="handleData(props.row)" v-if="root_xgzp"></el-button>
          <el-button type="danger" icon="el-icon-delete" circle @click="deletelist(props.row)" v-if="root_sczp"></el-button>

        </span>
        <span v-else>
          {{props.formattedRow[props.column.field]}}
        </span>
      </template>
      <div slot="table-actions">
        <el-button type="primary" @click="creatData()" v-if="root_xjzp">新建</el-button>
      </div>
    </vue-good-table>
    <!-- 设备详情 -->
    <el-dialog title="装配清单" :visible.sync="dialogFormVisible">
        <el-form :model="form">
          <el-form-item label="清单名称" :label-width="formLabelWidth">
            <el-input v-model="dialogFormdata.listname"  auto-complete="off"></el-input>
          </el-form-item>
          <el-form-item label="其他说明" :label-width="formLabelWidth">
            <el-input v-model="dialogFormdata.description" auto-complete="off"></el-input>
          </el-form-item>
        </el-form>
        <div slot="footer" class="dialog-footer">
          <el-button @click="dialogFormVisible = false">取 消</el-button>
          <el-button type="primary" @click="updateDataInfo()">确 定</el-button>
        </div>
    </el-dialog>
     <!-- 装配清单记录 -->
    <el-dialog title="清单详情" :visible.sync="dialogTableVisible">
        <el-form :model="form">
          <vue-good-table 
            :columns="checkcolumns" 
            :rows="checkrows" 
            @on-column-filter="selectionChanged"
            :search-options="{enabled: true}"
            :pagination-options="{enabled: true,mode: 'records',perPage: 5,perPageDropdown: [5],dropdownAllowAll: false,}"
          />
        </el-form>
    </el-dialog>
  </div>

</template>

<script>
import { VueGoodTable } from "vue-good-table"
import axios from 'axios'
export default {
  name: "Listoutshow",
  data() {
    return {
      dialogFormVisible: false,
      dialogTableVisible: false,
      root_xjzp:false,
      root_sczp:false,
      root_xgzp:false,
      form: {
        name: "",
        region: "",
        date1: "",
        date2: "",
        delivery: false,
        type: [],
        resource: "",
        desc: ""
      },
      dialogFormdata: {},
      formLabelWidth: "120px",
      searchItem: "",
      value1:'',
      columns: [
        {
          label: "序号",
          field: "number",
        },
        {
          label: "清单名称",
          field: "listname"
        },
        {
          label: "清单说明",
          field: "description"
        },
        {
          label: "操作",
          field: "operate"
        }
      ],
      // options: [{
      //     value: '0',
      //     label: '终端设备'
      //   }, {
      //     value: '1',
      //     label: '非终端设备'
      //   }],
        value:'',
      rows: [],
      checkcolumns: [
        {
          label: "序号",
          field: "num",
        },
        {
          label: "部件名称",
          field: "name"
        },
        {
          label: "图号",
          field: "figure_number"
        },
        {
          label: "规格",
          field: "child_material"
        },
        {
          label: "数量",
          field: "count"
        }
      ],
      checkrows: []
    };
  },
  components: {
    VueGoodTable
  },
  created () {
    this.getDataInfo()
    this.getroot_apply()
  },
  methods: {
    // 新建设备
    creatData () {
      this.dialogFormVisible = true
      this.dialogFormdata = {}
    },
    updateDataInfo () {
      this.dialogFormVisible = false
      // console.log(this.dialogFormdata)
      //检测是否存在list
      if(this.dialogFormdata.id) {
        var fd = new FormData()
        fd.append("flag",'update')
        fd.append("id",this.dialogFormdata.id)
        fd.append("listname",this.dialogFormdata.listname)
        fd.append("description",this.dialogFormdata.description)
        // console.log(fd)
        axios.post(`${this.baseURL}/listout/creatlist.php`,fd).then(this.creatRefresh)
        // this.$message({
        //   type: 'success',
        //   message: '保存成功!'
        // });
      }else{
         var fd = new FormData()
        fd.append("flag",'new')
        fd.append("listname",this.dialogFormdata.listname)
        fd.append("description",this.dialogFormdata.description)
        // console.log(fd)
        axios.post(`${this.baseURL}/listout/creatlist.php`,fd).then(this.creatRefresh)

      }
        // this.$message({
        //   type: 'error',
        //   message: '新建失败!'
        // });
    },
    creatRefresh(res) {
      // console.log(res)
      console.log(res.data['state'])
      if(res.data['state']=="success"){
          this.$message({
          type: 'success',
          message: '成功!'
        });
      }
      else if(res.data['state']=="exist"){
         this.$message({
          type: 'error',
          message: '已存在!'
        });
      }
      else if(res.data['state']=="nothing"){
         this.$message({
          type: 'success',
          message: '无更改!'
        });
      }
      else{
        this.$message({
          type: 'error',
          message: '失败!'
        });
      }
      this.getDataInfo()
    },
    // 获取设备list
    getDataInfo () {
      axios.post(`${this.baseURL}/listout/listshow.php`).then(this.getDataInfoSucc)
    },
    getDataInfoSucc (res){
      // console.log(res.data.data)
      res = res.data
      if(res.success && res.data){
        this.rows = []
        this.rows = res.data
        // console.log(this.rows)
      }
    },
    selectionChanged(params) {
      console.log(params.columnFilters);
    },
    handleClose(done) {
      this.$confirm("确认关闭？")
        .then(_ => {
          done();
        })
        .catch(_ => {});
    },
    // 删除设备
    deletelist(row) {
      // console.log(id)
        this.$confirm('将删除该清单, 是否继续?', '提示', {
          confirmButtonText: '确定',
          cancelButtonText: '取消',
          type: 'warning'
        }).then(() => {
          // this.$message({
          //   type: 'success',
          //   message: '删除成功!'
          // });
          var fd = new FormData()
          fd.append("id",row.id)
          fd.append("listname",row.listname)
          axios.post(`${this.baseURL}/listout/list_del.php`,fd).then(this.creatRefresh)
        }).catch(() => {
          this.$message({
            type: 'info',
            message: '已取消删除'
          });
        });
      },
    // 查看修改详情
    handleData(row) {
      this.dialogFormdata = {}
      this.dialogFormdata = row
      this.dialogFormVisible = true
    },
    // // 点检记录异步数据获取
    handleTabledata(row) {
      // console.log(row.id)
      var fd = new FormData()
      this.checkrows = []
      fd.append("id",row.id)
      fd.append("flag",'detail')
      axios.post(`${this.baseURL}/listout/creatlist.php`,fd).then(this.getcheckDataSucc)
      this.dialogTableVisible = true
    },
    getcheckDataSucc(res) {
      // console.log(res.data)
      res = res.data
      // console.log(res.data)
      if (res.data && res.success){
        this.checkrows = res.data
      }
    },
    //二维码
    listQRcode(row){
      // 清空缓存
      sessionStorage.removeItem('id')
      sessionStorage.removeItem('listname')
      // sessionStorage.removeItem('number')
      //缓存浏览器
      // sessionStorage.setItem('id',row.id)
      sessionStorage.setItem('listname',row.listname)
      sessionStorage.setItem('id',row.id)
      console.log(row.listname)
      // sessionStorage.setItem('number',row.number)
      // //打开生成二维码页面
      window.open('#/listoutQRcode', '_blank');
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
        this.getroot("31_XJZP").then(function(res){
          that.root_xjzp=res;
        });
        this.getroot("31_XGZP").then(function(res){
          that.root_xgzp=res;
        });
        this.getroot("31_SCZP").then(function(res){
          that.root_sczp=res;
        });
      }
  }
};
</script>

<style scoped>
</style>
