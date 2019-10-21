<template>
  <div class="crumbs">
    <!-- <div class="crumbs">
            <el-breadcrumb separator="/">
                <el-breadcrumb-item><i class="el-icon-message"></i> 消息通知</el-breadcrumb-item>
            </el-breadcrumb>
        </div>
        <div class="container">
            <el-header style="text-align: right; font-size: 12px; height: 20px">
                <el-button type="primary" @click="canupload = true" icon="el-icon-edit" circle></el-button>
            </el-header>
            <el-tabs v-model="message">
                <el-tab-pane :label="`未读消息(${unread.length})`" name="first">
                    <el-table :data="unread" :show-header="false" style="width: 100%">
                        <el-table-column>
                            <template slot-scope="scope">
                                <span class="message-title">{{scope.row.title}}</span>
                            </template>
                        </el-table-column>
                        <el-table-column prop="date" width="180"></el-table-column>
                        <el-table-column width="120">
                            <template slot-scope="scope">
                                <el-button size="small" @click="handleRead(scope.row)">标为已读</el-button>
                            </template>
                        </el-table-column>
                    </el-table>
                    <div class="handle-row">
                        <el-button type="danger" @click="allRead()">全部标为已读</el-button>
                    </div>
                </el-tab-pane>
                <el-tab-pane :label="`已读消息(${read.length})`" name="second">
                    <template v-if="message === 'second'">
                        <el-table :data="read" :show-header="false" style="width: 100%">
                            <el-table-column>
                                <template slot-scope="scope">
                                    <span class="message-title">{{scope.row.title}}</span>
                                </template>
                            </el-table-column>
                            <el-table-column prop="date" width="150"></el-table-column>
                            <el-table-column width="120">
                                <template slot-scope="scope">
                                    <el-button type="danger" @click="handleDel(scope.row)">删除</el-button>
                                </template>
                            </el-table-column>
                        </el-table>
                    </template>
                </el-tab-pane>
                <el-tab-pane :label="`全部消息(${recycle.length})`" name="third">
                    <template v-if="message === 'third'">
                        <el-table :data="recycle" :show-header="false" style="width: 100%">
                            <el-table-column>
                                <template slot-scope="scope">
                                    <span class="message-title">{{scope.row.title}}</span>
                                </template>
                            </el-table-column>
                            <el-table-column prop="date" width="150"></el-table-column>
                        </el-table>
                    </template>
                </el-tab-pane>
            </el-tabs>
            <el-dialog title="消息编辑" :visible.sync="canupload">
                <h4>消息编辑人：admin</h4>
            <el-input
                type="textarea"
                :rows="5"
                placeholder="请输入内容"
                v-model="textarea">
                </el-input>
                <el-checkbox-group v-model="checkList">
                    <el-checkbox label="全部"></el-checkbox>
                    <el-checkbox label="车间"></el-checkbox>
                    <el-checkbox label="质保部"></el-checkbox>
                    <el-checkbox label="工艺部"></el-checkbox>
                    <el-checkbox label="计划部"></el-checkbox>
                    <el-checkbox label="销售部"></el-checkbox>
                </el-checkbox-group>
            </el-dialog>
    </div>-->
    <el-tabs v-loading="loading" v-model="activeName" type="card">
      <el-tab-pane :label="`未读消息(${unread.length})`" name="first">
        <el-form :inline="true" class="el-form1">
          <el-form-item>
            <el-input placeholder="输入关键字" v-model="filterText" style="width:250px;height:35px"></el-input>
            <el-button type="primary" @click="handleFifter()">查询</el-button>
            <el-button type="primary" @click="reload()">重置</el-button>
            <el-button type="danger" @click="allRead()">全部标为已读</el-button>
            <el-button type="primary" @click="DateTimeShow = !DateTimeShow">更多</el-button>
          </el-form-item>
        </el-form>
        <div v-show="DateTimeShow">
          <DateTime ref="Time"></DateTime>
          <el-button type="primary" @click="selectUnreadTime()">按时间查询</el-button>
        </div>
        <el-table ref="filterTable" :data="unread" style="width: 100%" >
          <el-table-column prop="address" label="部件信息" :formatter="formatter"></el-table-column>
          <el-table-column
            prop="workshop"
            label="车间"
            width="100"
            :filters="WorkshopBox"
            :filter-method="filterRoute"
            column-key="WorkshopBox"
            filter-placement="bottom-end"
          ></el-table-column>
          <el-table-column
            prop="route"
            label="工序"
            width="100"
            filter-placement="bottom-end"
            :filters="RouteClass"
            :filter-method="filterHandler"
            column-key="RouteClass"
          ></el-table-column>
          <el-table-column
            prop="cuser"
            label="负责人"
            width="100"
            filter-placement="bottom-end"
            :filters="WorkcuserBox"
            :filter-method="filterRoute"
            column-key="WorkcuserBox"
          ></el-table-column>
          <el-table-column
            prop="state"
            label="状态"
            width="100"
            :filters="WorkstateBox"
            :filter-method="filterState"
            column-key="WorkstateBox"
            filter-placement="bottom-end"
          ></el-table-column>
          <el-table-column prop="date" label="日期" sortable width="180"></el-table-column>
          <el-table-column width="120">
            <template slot-scope="scope">
              <el-button size="small" type="primary" @click="handleRead(scope.row)">标为已读</el-button>
            </template>
          </el-table-column>
        </el-table>
        <div class="handle-row"></div>
      </el-tab-pane>

      <el-tab-pane :label="`已读消息(${read.length})`" name="second">
        <el-form :inline="true" class="el-form1">
          <el-form-item>
            <el-input placeholder="输入关键字" v-model="filterTextRead" style="width:250px;height:35px"></el-input>
            <el-button type="primary" @click="handleFifterRead()">查询</el-button>
            <el-button type="primary" @click="reload()">重置</el-button>
             <el-button type="primary" @click="DateTimeShow1 = !DateTimeShow1">更多</el-button>
          </el-form-item>
        </el-form>
         <div v-show="DateTimeShow1">
          <DateTime ref="Time1"></DateTime>
          <el-button type="primary" @click="selectReadTime()">按时间查询</el-button>
        </div>
        <el-table ref="filterTable" :data="read" style="width: 100%">
          <!-- <el-table-column prop="name" label="" width="180"></el-table-column> -->
          <el-table-column prop="address" label="部件信息" :formatter="formatter"></el-table-column>
           <el-table-column
            prop="workshop"
            label="车间"
            width="100"
            :filters="WorkshopBox1"
            :filter-method="filterRoute"
            column-key="WorkshopBox1"
            filter-placement="bottom-end"
          ></el-table-column>
          <el-table-column
            prop="route"
            label="工序"
            width="100"
            filter-placement="bottom-end"
            :filters="RouteClass"
            :filter-method="filterHandler"
            column-key="RouteClass"
          ></el-table-column>
          <el-table-column
            prop="cuser"
            label="负责人"
            width="100"
            filter-placement="bottom-end"
            :filters="WorkcuserBox1"
            :filter-method="filterRoute"
            column-key="WorkcuserBox1"
          ></el-table-column>
          <el-table-column
            prop="state"
            label="状态"
            width="100"
            :filters="WorkstateBox1"
            :filter-method="filterState"
            column-key="WorkstateBox1"
            filter-placement="bottom-end"
          ></el-table-column>
          <el-table-column prop="date" label="日期" sortable width="180"></el-table-column>
          <el-table-column width="120">
            <template slot-scope="scope">
              <el-button size="big" type="danger" @click="handleDel(scope.row)">删除消息</el-button>
            </template>
          </el-table-column>
        </el-table>
        <div class="handle-row">
          <el-button type="danger" @click="allDel()">删除全部消息</el-button>
        </div>
      </el-tab-pane>

      <!-- <el-tab-pane :label="`全部消息(${recycle.length})`" name="third">
        <el-table ref="filterTable" :data="recycle" style="width: 100%">
          <el-table-column prop="address" label="部件信息" :formatter="formatter"></el-table-column>
          <el-table-column
            prop="workshop"
            label="车间"
            width="100"
            :filters="[{ text: 'K开料车间', value: 'K开料车间' }, { text: 'TK开料车间', value: 'TK开料车间' }, { text: '安装S', value: '安装S' }, { text: '玻璃钢F', value: '玻璃钢F' }, { text: '电气G', value: '电气G' }, { text: '机加T', value: '机加T' }, { text: '结构L', value: '结构L' }, { text: '探伤', value: '探伤' }, { text: '外协W', value: '外协W' }]"
            :filter-method="filterRoute"
            filter-placement="bottom-end"
          ></el-table-column>
          <el-table-column
            prop="route"
            label="工序"
            width="100"
            filter-placement="bottom-end"
          ></el-table-column>
          <el-table-column
            prop="cuser"
            label="负责人"
            width="100"
            filter-placement="bottom-end"
          ></el-table-column>
          <el-table-column
            prop="state"
            label="状态"
            width="100"
            :filters="[ { text: '就工', value: '0' },{ text: '完工', value: '1' }]"
            :filter-method="filterState"
            filter-placement="bottom-end"
          ></el-table-column>
          <el-table-column prop="date" label="日期" sortable width="180"></el-table-column>
        </el-table>
      </el-tab-pane> -->
    </el-tabs>
  </div>
</template>

<script>
import { Loading } from 'element-ui';
import DateTime from './DateTimePicker'
import axios from "axios";
export default {
  inject: ["reload"],
  name: "tabs",
  components:{
    DateTime
  },
  data() {
    return {
      loading: true,
      activeName: "first",
      canupload: false,
      filterText: "",
      filterTextRead: "",
      showHeader: false,
      checkList: ["全部", "车间"],
      textarea: [],
      unread: [],
      read: [],
      recycle: [],
      WorkshopBox: [],
      WorkshopBox1: [],
      WorkstateBox: [],
      WorkstateBox1: [],
      WorkcuserBox: [],
      WorkcuserBox1: [],
      DateTimeShow:false,
      DateTimeShow1:false,
      RouteClass:[
        {text:"K",value:"K"},
        {text:"K坡",value:"K坡"},
        {text:"S安装补贴",value:"S安装补贴"},
        {text:"S玻璃钢",value:"S玻璃钢"},
        {text:"S厂检",value:"S厂检"},
        {text:"S电气",value:"S电气"},
        {text:"S调试",value:"S调试"},
        {text:"S钢结构",value:"S钢结构"},
        {text:"S国（省）检",value:"S国（省）检"},
        {text:"S派人维修",value:"S派人维修"},
        {text:"S移交客户",value:"S移交客户"},
        {text:"S座舱",value:"S座舱"},
        {text:"F成型",value:"F成型"},
        {text:"F翻模",value:"F翻模"},
        {text:"F模具",value:"F模具"},
        {text:"F喷涂",value:"F喷涂"},
        {text:"F装配",value:"F装配"},
        {text:"M木工",value:"M木工"},
        {text:"GS",value:"GS"},
        {text:"G接线",value:"G接线"},
        {text:"G装灯",value:"G装灯"},
        {text:"G装箱",value:"G装箱"},
        {text:"T粗",value:"T粗"},
        {text:"T淬",value:"T淬"},
        {text:"T调",value:"T调"},
        {text:"T发黑",value:"T发黑"},
        {text:"T焊",value:"T焊"},
        {text:"T划线",value:"T划线"},
        {text:"T坡",value:"T坡"},
        {text:"T退",value:"T退"},
        {text:"T线",value:"T线"},
        {text:"T正火",value:"T正火"},
        {text:"T装",value:"T装"},
        {text:"TK",value:"TK"},
        {text:"IA",value:"IA"},
        {text:"IA1",value:"IA1"},
        {text:"IB",value:"IB"},        
        {text:"ID",value:"ID"},  
        {text:"IG",value:"IG"},  
        {text:"IS",value:"IS"},  
        {text:"I钻",value:"I钻"},  
        {text:"LK",value:"LK"},  
        {text:"L焊",value:"L焊"}, 
        {text:"L转",value:"L转"},  
        {text:"L装",value:"L装"},  
        {text:"J探",value:"J探"},   

      ],
    };
  },
  created() {
    this.getDataTime();
    setTimeout(() => {
      this.getDataUnread();
    }, 1500);

    this.getDataRead();
    this.getDataRecycle();
  },
  methods: {
    // 过滤查询
    handleFifter() {
      var fd = new FormData();
      fd.append("flag", "Search");
      fd.append("modid", this.filterText);
      var department = localStorage.getItem("ms_department");
      fd.append("department", department);
      axios.post(`${this.baseURL}/tabs.php`, fd).then(unread => {
        //ES6写法
        unread = unread.data;
        if (unread.success && unread.data) {
          this.unread = [];
          this.unread = unread.data;
        }else{
          this.$message({
            showClose: true,
            message: '暂无数据',
            type: 'error'
          })
        }
      });
    },

    handleFifterRead() {
      var fd = new FormData();
      fd.append("flag", "SearchRead");
      fd.append("modid", this.filterTextRead);
      var department = localStorage.getItem("ms_department");
      fd.append("department", department);
      axios.post(`${this.baseURL}/tabs.php`, fd).then(read => {
        //ES6写法
        read = read.data;
        if (read.success && read.data) {
          this.read = [];
          this.read = read.data;
        }else{
          this.$message({
            showClose: true,
            message: '暂无数据',
            type: 'error'
          })
        }
      });
    },

    resetDateFilter() {
      this.$refs.filterTable.clearFilter("date");
    },
    clearFilter() {
      this.$refs.filterTable.clearFilter();
    },
    formatter(row, column) {
      return row.address;
    },
    filterTag(value, row) {
      return row.tag === value;
    },
    filterState(value, row) {
      return row.state === value;
    },
    filterRoute(value, row) {
      return row.workshop === value;
    },
    // filterRoute(value, row) {
    //   //   return row.state === value;
    //   if (row.route.indexOf(value) !== -1) {
    //     return value;
    //   }
    // },
    filterHandler(value, row, column) {
      const property = column["property"];
      return row[property] === value;
    },

    // 获取逾期消息
    getDataTime() {
      var fd = new FormData();
      fd.append("flag", "Overdue");
      axios.post(`${this.baseURL}/tabs.php`, fd).then(Overdue => {
        //ES6写法
        Overdue = Overdue.data;
        if (Overdue.success && Overdue.data) {
          this.Overdue = [];
          this.Overdue = Overdue.data;
        }
      });
    },

    // 获取未读
    getDataUnread() {
      var fd = new FormData();
      fd.append("flag", "Unread");
      var department = localStorage.getItem("ms_department");
      fd.append("department", department);
      axios.post(`${this.baseURL}/tabs.php`, fd).then(unread => {
        //ES6写法
        unread = unread.data;
        if (unread.success && unread.data) {
          this.unread = [];
          this.unread = unread.data;
          this.loading = false;
          let length1 = unread.WorkshopBox.length;
          for(let i=0; i < length1; i++) {
            if(unread.WorkshopBox[i].f5!==null&&unread.WorkshopBox[i].f5!==''){
              this.WorkshopBox.push({text:unread.WorkshopBox[i].f5,value:unread.WorkshopBox[i].f5})
            }  
          }
          let length2 = unread.WorkstateBox.length;
          for(let i=0; i < length2; i++) {
            if(unread.WorkstateBox[i].f6!==null&&unread.WorkstateBox[i].f6!==''){
              this.WorkstateBox.push({text:unread.WorkstateBox[i].f6,value:unread.WorkstateBox[i].f6})
            }  
          }
          let length3 = unread.WorkcuserBox.length;
          for(let i=0; i < length2; i++) {
            if(unread.WorkcuserBox[i].f6!==null&&unread.WorkcuserBox[i].f6!==''){
              this.WorkcuserBox.push({text:unread.WorkcuserBox[i].f7,value:unread.WorkcuserBox[i].f7})
            }  
          }
        }
      });
    },
    getDataInfoSucc(res) {
      res = res.data;
      if (res.success && res.rows) {
        this.rows = res.rows;
      }
    },
    //获取已读
    getDataRead() {
      this.WorkshopBox1 = [];
      this.WorkstateBox1 = [];
      var fd = new FormData();
      fd.append("flag", "Read");
      var department = localStorage.getItem("ms_department");
      fd.append("department", department);
      axios.post(`${this.baseURL}/tabs.php`, fd).then(read => {
        //ES6写法
        read = read.data;
        if (read.success && read.data) {
          this.read = [];
          this.read = read.data;
          let length1 = read.WorkshopBox1.length;
          for(let i=0; i < length1; i++) {
            if(read.WorkshopBox1[i].f5!==null&&read.WorkshopBox1[i].f5!==''){
              this.WorkshopBox1.push({text:read.WorkshopBox1[i].f5,value:read.WorkshopBox1[i].f5})
            }
          }
          let length2 = read.WorkstateBox1.length;
          for(let i=0; i < length2; i++) {
            if(read.WorkstateBox1[i].f6!==null&&read.WorkstateBox1[i].f6!==''){
              this.WorkstateBox1.push({text:read.WorkstateBox1[i].f6,value:read.WorkstateBox1[i].f6})
            }  
          }
          let length3 = read.WorkcuserBox1.length;
          for(let i=0; i < length3; i++) {
            if(read.WorkcuserBox1[i].f7!==null&&read.WorkcuserBox1[i].f7!==''){
              this.WorkcuserBox1.push({text:read.WorkcuserBox1[i].f7,value:read.WorkcuserBox1[i].f7})
            }  
          }
        }
      });
    },
    //获取全部消息
    getDataRecycle() {
      var fd = new FormData();
      fd.append("flag", "Recycle");
      axios.post(`${this.baseURL}/tabs.php`, fd).then(recycle => {
        //ES6写法
        recycle = recycle.data;
        if (recycle.success && recycle.data) {
          this.recycle = [];
          this.recycle = recycle.data;
        }
      });
    },
    //进入已读
    handleRead(row) {
      const item = this.unread.splice(row, 1);
      this.read = item.concat(this.read);
      var fd = new FormData();
      fd.append("flag", "ReadIn");
      fd.append("id", row.id);
      axios.post(`${this.baseURL}/tabs.php`, fd).then(recycle => {
        //ES6写法
        recycle = recycle.data;
      });
      // setTimeout(() => {
      //   this.getDataRead()
      // }, 1000);
    },
    //全部进入已读
    allRead(row) {
      for (let i = 0; i < this.unread.length; i++) {
        var fd = new FormData();
        fd.append("flag", "allRead");
        fd.append("id", this.unread[i].id);
        axios.post(`${this.baseURL}/tabs.php`, fd).then(recycle => {
          //ES6写法
          recycle = recycle.data;
        });
      }
      const item = this.unread.splice(row);
      this.read = item.concat(this.read);
    },
    //删除
    handleDel(row) {
      const item = this.read.splice(row, 1);
      this.recycle = item.concat(this.recycle);
      var fd = new FormData();
      fd.append("flag", "RecycleIn");
      fd.append("id", row.id);
      axios.post(`${this.baseURL}/tabs.php`, fd).then(recycle => {
        //ES6写法
        recycle = recycle.data;
      });
    },
    //全部删除
    allDel(row) {
      for (let i = 0; i < this.read.length; i++) {
        var fd = new FormData();
        fd.append("flag", "allDel");
        fd.append("id", this.read[i].id);
        axios.post(`${this.baseURL}/tabs.php`, fd).then(recycle => {
          //ES6写法
          recycle = recycle.data;
        });
      }
      const item = this.read.splice(row);
    },
    selectUnreadTime(){
      this.WorkshopBox= [];
      this.WorkstateBox= [];
      this.WorkcuserBox= [];
      var fd = new FormData();
      fd.append("flag", "selectUnreadTime");
      fd.append("DateData", this.$refs.Time.DateData);
      axios.post(`${this.baseURL}/tabs.php`, fd).then(unread => {
        //ES6写法
        unread = unread.data;
        if (unread.success && unread.data) {
          this.unread = [];
          this.unread = unread.data;
          this.loading = false;
          let length1 = unread.WorkshopBox.length;
          for(let i=0; i < length1; i++) {
            if(unread.WorkshopBox[i].f5!==null&&unread.WorkshopBox[i].f5!==''){
              this.WorkshopBox.push({text:unread.WorkshopBox[i].f5,value:unread.WorkshopBox[i].f5})
            }  
          }
          let length2 = unread.WorkstateBox.length;
          for(let i=0; i < length2; i++) {
            if(unread.WorkstateBox[i].f6!==null&&unread.WorkstateBox[i].f6!==''){
              this.WorkstateBox.push({text:unread.WorkstateBox[i].f6,value:unread.WorkstateBox[i].f6})
            }  
          }
          let length3 = unread.WorkcuserBox.length;
          for(let i=0; i < length2; i++) {
            if(unread.WorkcuserBox[i].f6!==null&&unread.WorkcuserBox[i].f6!==''){
              this.WorkcuserBox.push({text:unread.WorkcuserBox[i].f7,value:unread.WorkcuserBox[i].f7})
            }  
          }
        }else{
          this.$message({
            showClose: true,
            message: '暂无数据',
            type: 'error'
          })
        }
      });
      // console.log(this.$refs.Time.DateData)
    },
    selectReadTime(){
      this.WorkshopBox1= [];
      this.WorkstateBox1= [];
      this.WorkcuserBox1= [];
      var fd = new FormData();
      fd.append("flag", "selectReadTime");
      fd.append("DateData", this.$refs.Time1.DateData);
      axios.post(`${this.baseURL}/tabs.php`, fd).then(read => {
        //ES6写法
        read = read.data;
        if (read.success && read.data) {
          this.read = [];
          this.read = read.data;
          let length1 = read.WorkshopBox1.length;
          for(let i=0; i < length1; i++) {
            if(read.WorkshopBox1[i].f5!==null&&read.WorkshopBox1[i].f5!==''){
              this.WorkshopBox1.push({text:read.WorkshopBox1[i].f5,value:read.WorkshopBox1[i].f5})
            }
          }
          let length2 = read.WorkstateBox1.length;
          for(let i=0; i < length2; i++) {
            if(read.WorkstateBox1[i].f6!==null&&read.WorkstateBox1[i].f6!==''){
              this.WorkstateBox1.push({text:read.WorkstateBox1[i].f6,value:read.WorkstateBox1[i].f6})
            }  
          }
          let length3 = read.WorkcuserBox1.length;
          for(let i=0; i < length3; i++) {
            if(read.WorkcuserBox1[i].f7!==null&&read.WorkcuserBox1[i].f7!==''){
              this.WorkcuserBox1.push({text:read.WorkcuserBox1[i].f7,value:read.WorkcuserBox1[i].f7})
            }  
          }
        }else{
          this.$message({
            showClose: true,
            message: '暂无数据',
            type: 'error'
          })
        }
      });
    }
  },
  destroyed() {
    clearTimeout(this.timer);
  },
  computed: {
    unreadNum() {
      return this.unread.length;
    }
  }
};
</script>

<style scoped>
/* * {
  margin: 0;
  padding: 0;
} */
.message-title {
  cursor: pointer;
}
.el-tabs--card {
  height: 892px;
  overflow: scroll;
}
.el-card__body {
  padding-bottom: 0 !important;
}
form {
  height: 34px !important;
  padding: 0;
}
</style>

