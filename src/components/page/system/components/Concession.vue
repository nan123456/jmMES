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
        </span>
        <span v-else>
          {{props.formattedRow[props.column.field]}}
        </span>
      </template>
      <div slot="table-actions">
        <!-- <el-button type="primary" @click="creatData()">新建</el-button>0 -->
      </div>
    </vue-good-table>
  </div>

</template>

<script>
import { VueGoodTable } from "vue-good-table"
import axios from 'axios'
export default {
  name: "Concession",
  data() {
    return {
      dialogFormVisible: false,
      dialogTableVisible: false,
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
          label: "工号",
          field: "pNumber"
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
          label: "让步数量",
          field: "backcount"
        },
        {
          label: "总数量",
          field: "count"
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
        // {
        //   label: "设备编号",
        //   field: "number",
        // },
        // {
        //   label: "设备名称",
        //   field: "name"
        // },
        // {
        //   label: "点检结果",
        //   field: "checkresult"
        // },
        // {
        //   label: "点检时间",
        //   field: "checkdate"
        // },
        // {
        //   label: "点检人",
        //   field: "checkperson"
        // }
      ],
      checkrows: []
    };
  },
  components: {
    VueGoodTable
  },
  created () {
    this.getDataInfo()
  },
  methods: {
    // 获取设备list
    getDataInfo () {
      var fd = new FormData()
      fd.append("flag",'concession')
      axios.post(`${this.baseURL}/system/process.php`,fd).then(this.getDataInfoSucc)
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
    getcheckDataSucc(res) {
      // console.log(res.data)
      res = res.data
      // console.log(res.data)
      if (res.data && res.success){
        this.checkrows = res.data
      }
    },
  }
};
</script>

<style scoped>
</style>
