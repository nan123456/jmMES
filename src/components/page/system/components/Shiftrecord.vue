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
  name: "Shiftrecord",
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
          label: "部件名称",
          field: "name"
        },
        {
          label: "图号",
          field: "figure_number"
        },
        {
          label: "原工单",
          field: "pNumber"
        },
        {
          label: "被调工单",
          field: "shiftpNumber"
        },
        {
          label: "被调数量",
          field: "shiftcount"
        },
        {
          label: "总数量",
          field: "count"
        }
      ],
        value:'',
      rows: [],
      checkcolumns: [
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
      fd.append("flag",'shift')
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
