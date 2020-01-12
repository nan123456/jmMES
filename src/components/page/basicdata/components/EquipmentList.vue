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
          <el-button type="primary"   @click="equipmentQRcode(props.row)">点检二维码</el-button>
          <el-button type="primary"   @click="equipmentBarcode(props.row)">条形码</el-button>
          <el-button type="primary"   @click="handleTabledata(props.row.id)">查看月检表</el-button>
          <el-button type="primary" icon="el-icon-edit" circle @click="handleData(props.row)"></el-button>
          <el-button type="danger" icon="el-icon-delete" circle @click="deleteEquipment(props.row)" ></el-button>
          
        </span>
        <span v-else>
          {{props.formattedRow[props.column.field]}}
        </span>
      </template>
      <div slot="table-actions">
        <el-button type="primary" @click="creatData()">新建</el-button>
        <!-- <el-button type="primary">导入</el-button> -->
      </div>
    </vue-good-table>
    <!-- 设备详情 -->
    <el-dialog title="信息详情" :visible.sync="dialogFormVisible">
        <el-form>
          <el-form-item label="设备编号" :label-width="formLabelWidth">
            <el-input maxlength='20' v-model="dialogFormdata.number"  auto-complete="off" placeholder="最大长度20位"></el-input>
          </el-form-item>
          <el-form-item label="设备类型" :label-width="formLabelWidth">
              <el-select v-model="dialogFormdata.type" placeholder="请选择">
                <el-option
                  v-for="item in typeOptions"
                  :key="item.value"
                  :label="item.value"
                  :value="item.value">
                </el-option>
              </el-select>
          </el-form-item>
          <el-form-item label="设备名称" :label-width="formLabelWidth">
            <el-input maxlength='20' v-model="dialogFormdata.name" auto-complete="off" placeholder="最大长度20位"></el-input>
          </el-form-item>
          <el-form-item label="设备型号" :label-width="formLabelWidth">
            <el-input maxlength='50' v-model="dialogFormdata.typenumber" auto-complete="off" placeholder="最大长度50位"></el-input>
          </el-form-item>
          <el-form-item label="设备状态"  :label-width="formLabelWidth">
            <el-input maxlength='20' v-model="dialogFormdata.state" auto-complete="off" placeholder="最大长度20位"></el-input>
          </el-form-item>
          <el-form-item label="车间名称" :label-width="formLabelWidth">
            <el-input maxlength='20' v-model="dialogFormdata.workcenter" auto-complete="off" placeholder="最大长度20位"></el-input>
          </el-form-item>
          <el-form-item label="点检要求" :label-width="formLabelWidth">
            <el-input maxlength='20' v-model="dialogFormdata.checkrequest" auto-complete="off" placeholder="最大长度20位"></el-input>
          </el-form-item>
          <el-form-item label="点检位置描述" :label-width="formLabelWidth">
            <el-input maxlength='20' v-model="dialogFormdata.tallyposition" auto-complete="off" placeholder="最大长度20位"></el-input>
          </el-form-item>
          <el-form-item label="点检周期" :label-width="formLabelWidth">
            <el-input maxlength='20' v-model="dialogFormdata.tallycycle" auto-complete="off" placeholder="最大长度20位"></el-input>
          </el-form-item>
          <el-form-item label="设备" :label-width="formLabelWidth">
            <el-select v-model="dialogFormdata.terminal" placeholder="请选择">
              <el-option
                v-for="item in options"
                :key="item.value"
                :label="item.label"
                :value="item.value">
              </el-option>
            </el-select>
          </el-form-item>
        </el-form>
        <div slot="footer" class="dialog-footer">
          <el-button @click="dialogFormVisible = false">取 消</el-button>
          <el-button type="primary" @click="updateDataInfo()">确 定</el-button>
        </div>
    </el-dialog>
    <!-- 月检表列表 -->
    <el-dialog title="月检表列表" :visible.sync="dialogTableVisible">
        <el-button type="primary" class="new_check_btn" @click="newCheckTable()">新建月检表</el-button>  
        <el-form :model="form">
          <vue-good-table 
            :columns="checkcolumns" 
            :rows="checkrows" 
            @on-column-filter="selectionChanged"
            :search-options="{enabled: true}"
            :pagination-options="{enabled: true,mode: 'records',perPage: 5,perPageDropdown: [5],dropdownAllowAll: false,}">
            <template slot="table-row" slot-scope="props">
              <span v-if="props.column.field == 'operate'">
                <el-button type="primary"   @click="examineCkeckTable(props.row.id)">查看</el-button>  
                <el-button type="danger"   @click="deleteCkeckTable(props.row.id)">删除</el-button>              
              </span>
              <span v-else>
                {{props.formattedRow[props.column.field]}}
              </span>
            </template>            
          </vue-good-table>
        </el-form>
        <div slot="footer" class="dialog-footer">
          <el-button type="danger" @click="closeList()">关 闭</el-button>
        </div>
        <el-form class="newCheckForm" label-position="right" v-if="newCheckTableShow" label-width="180px">
            <el-form-item  class="newCheckFormMonth"  label="请选择点检月份">
                <el-date-picker
                  style="width:250px"
                  v-model="newFormMonth"
                  type="month"
                  placeholder="选择月"
                  value-format="yyyy-MM">
                </el-date-picker>
            </el-form-item>
            <el-form-item  class="newCheckFormUser"  label="操作者">
                <el-input style="width:250px" maxlength='20' v-model="newFormUser" placeholder="最大长度20位"></el-input>
            </el-form-item>
            <el-form-item  class="newCheckFormGroup"  label="班组">
                <el-input style="width:250px" maxlength='20' v-model="newFormGroup" placeholder="最大长度20位"></el-input>
            </el-form-item>
            <el-button class="newCheckTableCancelBtn" type="danger" @click="newCheckTableCancel()">取 消</el-button>
            <el-button class="newCheckTableSaveBtn" type="primary" @click="saveNewCheckTable(equipmentID)">确 定</el-button>
        </el-form>
        <Checklist  v-if="checkTableShow" class="checklist" :checkTableId='checkTableId' @listen='listenChild'></Checklist>
    </el-dialog>
  </div>  
</template>

<script>
import { VueGoodTable } from "vue-good-table"
import axios from 'axios'
import Checklist from './Checklist'
export default {
  name: "EquipmentList",
  components: {
    VueGoodTable,
    Checklist
  },
  props:{
    name:String
  },
  data() {
    return {
      dialogFormVisible: false,
      dialogTableVisible: false,
      checkTableShow:false,
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
      dialogFormdata:{
        number:'',
        type:'',
        name:'',
        typenumber:'',
        state:'',
        workcenter:'',
        checkrequest:'',
        tallyposition:'',
        tallycycle:'',
        terminal:''
      },
      formLabelWidth: "120px",
      searchItem: "",
      value1:'',
      typeOptions: [{value: '普通车床'}, {value: '数控车床'}, {value: '卧式铣镗床'}, {value: '钻床'}, {value: '锯床'}, {value: '叉车'}, {value: '打砂机'},
       {value: '电火花切割机'}, {value: '铣床'}, {value: '滚丝机'}, {value: '中、高频加热机'}, {value: '吊车'}, {value: '汽车起重机'}, {value: '滚齿机'}, 
       {value: '剪板机'}, {value: '牛头刨床'}, {value: '磨床'}, {value: '数控切割机'}, {value: '龙门刨床'}, {value: '液压机'}, {value: '镗床'}, 
       {value: '电梯'}, {value: '卷板机'}],      
      columns: [
        {
          label: "设备编号",
          field: "number",
        },
        {
          label: "设备名称",
          field: "name"
        },
        {
          label: "设备类型",
          field: "type"
        },
        {
          label: "设备型号",
          field: "typenumber"
        },
        {
          label: "设备状态",
          field: "state"
        },
        {
          label: "车间名称",
          field: "workcenter"
        },
        {
          label: "点检要求",
          field: "checkrequest"
        },{
          label: "点检位置描述",
          field: "tallyposition"
        },{
          label: "点检周期",
          field: "tallycycle"
        },
        {
          label: "操作",
          field: "operate"
        }
      ],
      options: [{
          value: '0',
          label: '终端设备'
        }, {
          value: '1',
          label: '非终端设备'
        }],
        value:'',
      rows: [],
      checkcolumns: [
        {
          label: "设备编号",
          field: "e_number",
        },
        {
          label: "设备类型名称",
          field: "e_name"
        },
        {
          label: "设备型号",
          field: "e_type"
        },
        {
          label: "点检月份",
          field: "year_month"
        },
        {
          label: "车间",
          field: "workshop"
        },
        {
          label: "班组",
          field: "group"
        },
        {
          label: "操作",
          field: "operate"          
        }
      ],
      checkrows: [],
      equipmentID:'',
      newCheckTableShow:false,
      newFormMonth:'',
      newFormUser:'',
      newFormGroup:'',
      checkTableId:''
    };
  },
  created () {
    this.getDataInfo()
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
      // 判断dialogFormdata.id是否存在，若存在说明是已有设备，若不存在则说明是新建设备
      if(this.dialogFormdata.id) {
        var fd = new FormData()
        fd.append("id",this.dialogFormdata.id)
        fd.append("number",(this.dialogFormdata.number==undefined||this.dialogFormdata.number==null)?'':this.dialogFormdata.number)
        fd.append("type",(this.dialogFormdata.type==undefined||this.dialogFormdata.type==null)?'':this.dialogFormdata.type)
        fd.append("typenumber",(this.dialogFormdata.typenumber==undefined||this.dialogFormdata.typenumber==null)?'':this.dialogFormdata.typenumber)
        fd.append("name",(this.dialogFormdata.name==undefined||this.dialogFormdata.name==null)?'':this.dialogFormdata.name)
        fd.append("state",(this.dialogFormdata.state==undefined||this.dialogFormdata.state==null)?'':this.dialogFormdata.state)
        fd.append("workcenter",(this.dialogFormdata.workcenter==undefined||this.dialogFormdata.workcenter==null)?'':this.dialogFormdata.workcenter)
        fd.append("checkrequest",(this.dialogFormdata.checkrequest==undefined||this.dialogFormdata.checkrequest==null)?'':this.dialogFormdata.checkrequest)
        fd.append("tallyposition",(this.dialogFormdata.tallyposition==undefined||this.dialogFormdata.tallyposition==null)?'':this.dialogFormdata.tallyposition)
        fd.append("tallycycle",(this.dialogFormdata.tallycycle==undefined||this.dialogFormdata.tallycycle==null)?'':this.dialogFormdata.tallycycle)
        fd.append("terminal",(this.dialogFormdata.terminal==undefined||this.dialogFormdata.terminal==null)?'':this.dialogFormdata.terminal)
        // console.log(fd)
        axios.post(`${this.baseURL}/basicdata/equipment_reserve.php`,fd).then(this.creatRefresh)
        this.$message({
        type: 'success',
        message: '保存成功!'
      });
      }else {
        console.log(this.dialogFormdata)
        // console.log(this.dialogFormdata.id)
        var fd = new FormData()
        fd.append("number",(this.dialogFormdata.number==undefined||this.dialogFormdata.number==null)?'':this.dialogFormdata.number)
        fd.append("type",(this.dialogFormdata.type==undefined||this.dialogFormdata.type==null)?'':this.dialogFormdata.type)
        fd.append("typenumber",(this.dialogFormdata.typenumber==undefined||this.dialogFormdata.typenumber==null)?'':this.dialogFormdata.typenumber)
        fd.append("name",(this.dialogFormdata.name==undefined||this.dialogFormdata.name==null)?'':this.dialogFormdata.name)
        fd.append("state",(this.dialogFormdata.state==undefined||this.dialogFormdata.state==null)?'':this.dialogFormdata.state)
        fd.append("workcenter",(this.dialogFormdata.workcenter==undefined||this.dialogFormdata.workcenter==null)?'':this.dialogFormdata.workcenter)
        fd.append("checkrequest",(this.dialogFormdata.checkrequest==undefined||this.dialogFormdata.checkrequest==null)?'':this.dialogFormdata.checkrequest)
        fd.append("tallyposition",(this.dialogFormdata.tallyposition==undefined||this.dialogFormdata.tallyposition==null)?'':this.dialogFormdata.tallyposition)
        fd.append("tallycycle",(this.dialogFormdata.tallycycle==undefined||this.dialogFormdata.tallycycle==null)?'':this.dialogFormdata.tallycycle)
        fd.append("terminal",(this.dialogFormdata.terminal==undefined||this.dialogFormdata.terminal==null)?'':this.dialogFormdata.terminal)
        // console.log(fd)
        axios.post(`${this.baseURL}/basicdata/equipment_reserve.php`,fd).then(this.creatRefresh)
        this.$message({
          type: 'success',
          message: '新建成功!'
        });
      }
      
    },
    creatRefresh(res) {
      // console.log(res)
      this.getDataInfo()
    },
    // 获取设备list
    getDataInfo () {
      axios.post(`${this.baseURL}/basicdata/equipment.php`).then(this.getDataInfoSucc)
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
    deleteEquipment(row) {
      // console.log(id)
        this.$confirm('将删除该设备, 是否继续?', '提示', {
          confirmButtonText: '确定',
          cancelButtonText: '取消',
          type: 'warning'
        }).then(() => {
          this.$message({
            type: 'success',
            message: '删除成功!'
          });
          var fd = new FormData()
          fd.append("id",row.id)
          axios.post(`${this.baseURL}/basicdata/equipment_del.php`,fd).then(this.creatRefresh)
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
    // 月检表数据获取
    handleTabledata(id) {
      this.equipmentID=id;
      var fd = new FormData()
      this.checkrows = []
      fd.append("flag",'getCheckList')
      fd.append("id",id)
      axios.post(`${this.baseURL}/basicdata/equipment_check.php`,fd).then(this.getcheckDataSucc)
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
    //点检二维码
    equipmentQRcode(row){
      // 清空缓存
      sessionStorage.removeItem('id')
      sessionStorage.removeItem('name')
      sessionStorage.removeItem('number')
      //缓存浏览器
      sessionStorage.setItem('id',row.id)
      sessionStorage.setItem('name',row.name)
      sessionStorage.setItem('number',row.number)
      //打开生成二维码页面
      window.open('#/equipmentQRcode', '_blank');
    },
    //条形码
    equipmentBarcode(row){
      // 清空缓存
      sessionStorage.removeItem('id')
      sessionStorage.removeItem('name')
      sessionStorage.removeItem('number')
      //缓存浏览器
      sessionStorage.setItem('id',row.id)
      sessionStorage.setItem('name',row.name)
      sessionStorage.setItem('number',row.number)
      //打开生成二维码页面
      window.open('#/equipmentBar', '_blank');
    },
    // 单月检表查看
    examineCkeckTable(id){
      this.checkTableShow=true;
      this.checkTableId=id;
    },
    //单月检表删除
    deleteCkeckTable(id){
      var that=this;
      this.$confirm('此操作将删除该月检表, 是否继续?', '提示', {
        confirmButtonText: '确定',
        cancelButtonText: '取消',
        type: 'warning'
      }).then(() => {
        var fd = new FormData();
        fd.append("flag",'deleteCheckTable')
        fd.append("id",id)
        axios.post(`${this.baseURL}/basicdata/equipment_check.php`,fd).then(function (res){
            if(res.data.res=='success'){
              that.$message({
                type: 'success',
                message: '删除成功!'
              });
              that.handleTabledata(that.equipmentID);
            }else{
              that.$message({
                type: 'info',
                message: '删除失败!'
              });
            }
          }
        ) 
      }).catch(() => {
        this.$message({
          type: 'info',
          message: '已取消删除'
        });          
      });            
    },
    // 新建月检表
    newCheckTable(){
      this.newCheckTableShow=true;
    },
    //关闭新建月检表
    newCheckTableCancel(){
      this.newCheckTableShow=false;
      this.newFormMonth='';
      this.newFormUser='';
      this.newFormGroup='';
    },
    //关闭月检表列表
    closeList(){
      this.dialogTableVisible = false;
      this.newCheckTableShow=false;
      this.newFormMonth='';
      this.newFormUser='';
      this.newFormGroup='';
    },
    saveNewCheckTable(id){
      var that=this;
      var newFormMonth=this.newFormMonth;
      var fd = new FormData();
      fd.append("flag",'saveNewCheckTable')
      fd.append("e_id",id)
      fd.append("year_month",this.newFormMonth)
      fd.append("e_user",this.newFormUser)
      fd.append("group",this.newFormGroup)
      axios.post(`${this.baseURL}/basicdata/equipment_check.php`,fd).then(function (res){
            if(res.data.res=='repetitive'){
                that.$message({
                  message: '该设备已存在此月份点检表',
                  type: 'error'
                });              
            }else if(res.data.res=='error'){
                that.$message({
                  message: '创建失败',
                  type: 'error'
                });               
            }else if(res.data.res=='success'){
                that.$message({
                  message: '创建成功',
                  type: 'success'
                });
                that.newCheckTableShow=false;
                that.newFormMonth='';
                that.newFormUser='';
                that.newFormGroup='';
                that.handleTabledata(that.equipmentID);                                
            }
        }
      )
    },
    //监听子部件
    listenChild(val){
      this.checkTableShow=false;
      this.checkTableId=0;
    }
  }
};
</script>

<style scoped>
  .new_check_btn{
    position: absolute;
    right: 20px;
    top: 20px;
    z-index: 5;
  }
  .new_input{
    width: 300px;
  }
  .newCheckForm{
    border: solid;
    background-color: white;
    position: absolute;
    z-index: 10;
    width: 550px;
    height: 230px;
    margin: auto;
    left: 0;
    right: 0;
    top: 0;
    bottom: 0;
  }
  .newCheckFormMonth{
    position: absolute;
    top: 20px;
  }
  .newCheckFormUser{
    position: absolute;
    top: 80px;
  }
  .newCheckFormGroup{
    position: absolute;
    top: 140px;
  }
  .newCheckTableCancelBtn{
    position: absolute;
    top: 190px;
    right: 100px;
  }
  .newCheckTableSaveBtn{
    position: absolute;
    top: 190px;
    right: 30px;
  }
  .checklist{
    width: 90%;
    height:600px;
    position: fixed;
    background: #fff;
    z-index: 9;
    margin: auto;
    left: 0;
    right: 0;
    top: -20px;
    bottom: 0;
  }
</style>