<template>
  <div>
    <vue-good-table :columns="columns" :rows="rows" :search-options="{enabled: true}">
      <template slot="table-row" slot-scope="props">
        <span v-if="props.column.field == 'operate'">
          <el-button type="primary" icon="el-icon-edit" circle @click="editTable(props.row)"></el-button>
          <el-button type="primary" icon="el-icon-tickets" circle @click="authority(props.row)"></el-button>
          <el-button type="danger" icon="el-icon-delete" circle @click="deleteStaff(props.row)"></el-button>
        </span>
        <span v-else>{{props.formattedRow[props.column.field]}}</span>
      </template>
      <div slot="table-actions">
        <el-button type="primary" @click="dialogFormVisible = true">新建</el-button>
        <el-button type="primary">导入</el-button>
      </div>
    </vue-good-table>
    <el-dialog title="职位设置" :visible.sync="dialogFormVisible">
      <el-form :model="form">
        <el-form-item label="工号" :label-width="formLabelWidth">
          <el-input auto-complete="off" v-model="gNum"></el-input>
        </el-form-item>
        <el-form-item label="姓名" :label-width="formLabelWidth">
          <el-input auto-complete="off" v-model="size"></el-input>
        </el-form-item>
        <el-form-item label="手机" :label-width="formLabelWidth">
          <el-input auto-complete="off" v-model="place"></el-input>
        </el-form-item>
        <el-form-item label="职位" :label-width="formLabelWidth">
          <el-select v-model="date">
            <el-option
              v-for="item in options1"
              :key="item.value"
              :label="item.label"
              :value="item.value"
            ></el-option>
          </el-select>
        </el-form-item>
      </el-form>
      <div slot="footer" class="dialog-footer">
        <el-button @click="dialogFormVisible = false">取 消</el-button>
        <el-button type="primary" @click="sureJob(props.row)">确 定</el-button>
      </div>
    </el-dialog>

    <el-dialog  title="显示模块" :visible.sync="Displaymodule">
      <el-form :model="form">
        <el-tree 
          :data="data" 
          :props="defaultProps" 
          show-checkbox 
          ref="tree"
          highlight-current
          node-key="value"></el-tree>
      </el-form>
      <div slot="footer" class="dialog-footer">
        <el-button @click="Displaymodule = false ">取 消</el-button>
        <el-button type="primary" @click="sureModule()">确 定</el-button>
      </div>
    </el-dialog>

    <el-dialog title="消息来源设置" :visible.sync="Msgmodule">
      <el-form :model="form">
        <el-form-item label="部门" :label-width="formLabelWidth">
          <el-select v-model="value">
            <el-option
              v-for="item in options"
              :key="item.value"
              :label="item.label"
              :value="item.value"
            ></el-option>
          </el-select>
        </el-form-item>
      </el-form>
      <div slot="footer" class="dialog-footer">
        <el-button @click="Msgmodule = false">取 消</el-button>
        <el-button type="primary" @click="Msgmodule = false">确 定</el-button>
      </div>
    </el-dialog>
  </div>
</template>

<script>
import axios from "axios"
import { VueGoodTable } from "vue-good-table"
export default {
  name: "AuthorityList",
  data() {
    return {
      dialogFormVisible: false,
      Displaymodule: false,
      Msgmodule: false,
      gNum:"",
      size:"",
      place:"",
      date:"",
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
      data: [
        {
          label:"系统首页",
          value:"1" 
        },
        {
          label: "制造模块",
          value:"13",
          children: [
            {
              label: "车间计划",
              value:"16"
            },
            {
              label: "电子看板",
              value:"17"
            },
            {
              label: "在建项目",
              value:"14",
              children:[
                {
                  label:"查看在建项目模块",
                  value:"14"
                },
                {
                  label:"导入在建项目",
                  value:"14_DRZJ"
                },
                {
                  label:"删除在建项目",
                  value:"14_SCZJ"
                }
              ]
            },
            {
              label: "已完成项目",
              value:"15"
            },
            {
              label: "发货清单",
              value:"32",
              children:[
                {
                  label:"查看发货清单模块",
                  value:"32"
                },
                {
                  label:"新建装配清单",
                  value:"31_XJZP"
                },
                {
                  label:"修改装配清单",
                  value:"31_XGZP"
                },
                {
                  label:"删除装配清单",
                  value:"31_SCZP"
                }
              ]
            }
          ]
        },
        {
          label: "工艺模块",
          value:"19",
          children: [
            {
              label: "工艺报表管理",
              value:"20",
              children:[
                {
                  label:"查看工艺报表模块",
                  value:"20"
                },
                {
                  label:"新建工艺卡",
                  value:"19_XJGYK"
                },
                {
                  label:"删除工艺卡",
                  value:"19_SCGYK"
                },
                {
                  label:"修改工艺卡",
                  value:"19_XGGYK"
                },
                {
                  label:"复制工艺卡",
                  value:"19_FZGYK"
                }
              ]
            },{
              label:"工艺卡模板",
              value:"21"
            }
          ]
        },{
          label: "检验模块",
          value:"22"
        },
        {
          label: "数据统计",
          value:"23",
          children:[
            {
              label: "数据归档",
              value:"25",
              children:[
                {
                  label:"查看数据归档模块",
                  value:"29"
                },
                {
                  label:"导出归档文件",
                  value:"29_DCGD"
                }
              ]
            },
            {
              label: "工时统计",
              value:"24"           
            }
          ]
        },
        {
          label: "系统设置",
          value:"26",
          children: [
            {
              label: "员工信息管理",
              value:"27"
            },
            {
              label: "操作日志",
              value:"28"
            },
            {
              label: "权限管理",
              value:"29"
            },
            {
              label: "注册新用户",
              value:"33",
            }
          ]
        }
      ],
      defaultProps: {
        children: "children",
        label: "label"
      },
      formLabelWidth: "120px",
      searchItem: "",
      value1: "",
      options: [
        {
          value: "销售部",
          label: "销售部"
        },
        {
          value: "计划部",
          label: "计划部"
        },
        {
          value: "工艺部",
          label: "工艺部"
        },
        {
          value: "车间",
          label: "车间"
        }
      ],
      value: "",
      props: {
        label: "name",
        children: "zones"
      },
      options1: [   //职位设置中的职位
        {
          value: "超级管理员",
          label: "超级管理员"
        },
        {
          value: "经理",
          label: "经理"
        },
        {
          value: "主管",
          label: "主管"
        },
        {
          value: "工人",
          label: "工人"
        }
      ],
      count: 1,
      columns: [
        {
          label: "工号",
          field: "specification"
        },
        {
          label: "姓名",
          field: "size"
        },
        { 
          label: "手机",
          field: "place"
        },
        {
          label: "职位",
          field: "date"
        },
        {
          label: "操作",
          field: "operate"
        }
      ],
      rows: [],
    };
  },
  components: {
    VueGoodTable
  },
  created() {
    this.getDataInfo()
  },
  methods: {
    getDataInfo() {
      var fd = new FormData()
      fd.append("flag","Select")
      axios.post(`${this.baseURL}/system/authoritylist.php`,fd).then((res)=> {  //ES6写法
        res = res.data
        // if (res.success && res.data) {
        this.rows = []
        this.rows = res.data
        // }

      })
    },
    // 删除员工
    deleteStaff(row) {
      this.$confirm("将删除该员工, 是否继续?", "提示", {
        confirmButtonText: "确定",
        cancelButtonText: "取消",
        type: "warning"
      })
        .then(() => {
          let fd = new FormData()
              fd.append("flag","del")
              fd.append("gNum",row.specification)
          axios.post(`${this.baseURL}/system/authoritylist.php`,fd).then(this.getDataInfo)
          this.$message({
            type: "success",
            message: "删除成功!"
          });
        })
        .catch(() => {
          this.$message({
            type: "info",
            message: "已取消删除"
          });
        });
    },
    //职位设置修改
    editTable(row) {
      // console.log(row)
      this.dialogFormVisible = true    //将模态框属性变成打开状态
      this.gNum = row.specification    
      this.size = row.size
      this.place = row.place
      this.date = row.date
    },
    //职位设置保存
    sureJob(){
      var fd = new FormData()
          fd.append("flag","Save")
          fd.append("gNum",this.gNum)
          fd.append("size",this.size)
          fd.append("place",this.place)
          fd.append("date",this.date)
      axios.post(`${this.baseURL}/system/authoritylist.php`,fd).then(function(res){
          // console.log(res)
          if(res.data.success ==="success"){
            alert("修改成功")
            location.reload()
          }else{
            alert("修改失败")
          }
      })
      
      this.dialogFormVisible = false    //关闭模态框
    },
    //显示模块设置
    authority(row){
      
      var storage = window.localStorage
      var gNum = row.specification
      var that = this
      storage.gNum = gNum
      var fd = new FormData()
          fd.append("flag","Showseemodule")
          fd.append("gNum",gNum)
          axios.post(`${this.baseURL}/system/authoritylist.php`,fd).then(function(res){
            if(res.data.success ==="success"){
              var seeModuleValue = res.data.seeModuleValue.split(",");
              console.log(seeModuleValue);
               that.$refs.tree.setCheckedKeys(seeModuleValue)
            }else{
              alert("错误") 
            }
        })
        // setTimeout(function () {
              that.Displaymodule = true
            // },00);    
             //点击执行打开显示模块设置模态框 
    },
    //显示模块设置内容修改
    sureModule(row){

      var seeModule = this.$refs.tree.getCheckedKeys()
      var storage=window.localStorage
      var gNum=storage.gNum
      var fd = new FormData()
          fd.append("flag","Seemodule")
          fd.append("seeModule",seeModule)
          fd.append("gNum",gNum)
          axios.post(`${this.baseURL}/system/authoritylist.php`,fd).then(function(res){
            // console.log(res)
            if(res.data.success ==="success"){
              alert("修改成功")
            }else{
              alert("修改失败")
            }
        })
      this.Displaymodule = false     //点击执行关闭显示模块设置模态框  
    }
  }
};
</script>

<style scoped>
.container{
    height: 85vh;
}
</style>
