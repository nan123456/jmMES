<template>
  <div>
    <el-button type="primary" @click="btn_reload()" class="btn_reload">重置</el-button>
    <vue-good-table
      v-if="show_goodtable"
      title="未检验部件"
      :columns="columns"
      :rows="rows"
      :pagination-options="{enabled: true,mode: 'records'}"
      :search-options="{enabled: true,trigger: 'enter',placeholder:'请输入查询内容并回车'}"
    >
      <template slot="table-row" slot-scope="props">
        <span v-if="props.column.field == 'mode'">
          <div>
            <el-button 
              type="primary"
              v-if="props.row.show_btn"
              @click="checkCard(props.row.figure_number,props.row.pNumber,'mode')"
              :name="props.row.figure_number"
            >
              查看工艺卡
            </el-button>
          </div>
        </span>
        <span v-if="props.column.field == 'check'">
          <div>
            <el-button 
              type="primary" 
              v-if="props.row.show_btn1"
              @click="checkCard(props.row.figure_number,props.row.pNumber,'check')"
              :name="props.row.figure_number"
            >
              查看工艺卡
            </el-button>
          </div>
        </span>
        <span v-if="props.column.field == 'heat'">
          <div>
            <el-button 
              type="primary"
              v-if="props.row.show_btn2"
              @click="checkCard(props.row.figure_number,props.row.pNumber,'heat')"
              :name="props.row.figure_number"
            >
              查看工艺卡
            </el-button>
          </div>
        </span>
        <span v-if="props.column.field == 'maching'">
          <div>
            <el-button 
              type="primary" 
              v-if="props.row.show_btn3"
              @click="checkCard(props.row.figure_number,props.row.pNumber,'mach')"
              :name="props.row.figure_number"
            >
              查看工艺卡
            </el-button>
          </div>
        </span>
        <span v-if="props.column.field == 'finish'">
          <!-- <el-button 
            type="primary"
            @click="print1(props.row)"
          >
            查看
          </el-button> -->
          <router-link
            :to="{path:'Photo',query: {url: props.row.photourl}}"
            :key="props.index"
            v-if="props.row.photourl!=''"
            target="_blank">
          <el-button 
            type="primary">
            查看
          </el-button></router-link>          
        </span>
        
        <span v-else-if="props.column.field == 'result'">
          <!-- <a 
            target="_blank"
            class="show_img"
            :key="props.index"
            v-if="props.row.show_img"
          > 点击查看</a> -->
          
          <router-link
            :to="{path:'Photo',query: {url: props.row.photourl}}"
            class="show_img"
            :key="props.index"
            v-if="props.row.photourl!=''"
            target="_blank"          
          ><el-button 
            type="primary">
            查看图片
          </el-button></router-link>
        </span>
        <span v-if="props.column.field == 'checkSituation'">
          <el-button 
            type="primary"
            @click="print()"
          >
            打印
          </el-button>
        </span>
        <span v-else>
          {{props.formattedRow[props.column.field]}}
        </span>
      </template>
    </vue-good-table>
    <examine-dialog :config="dialog"></examine-dialog>
    <!-- 工艺卡Dialog -->
    <el-dialog
      :title="Cardtitle"
      width="40%"
      :before-close="handleClose"
      :visible.sync="dialogVisible"
    >
    <div v-if="modeshow">
      <el-button type="primary" size="small" v-for="(item,key) in CardData.data" :key="key" @click="mode(item.contactId,item.selectedTreeNode)">工艺卡{{key+1}}</el-button>
    </div>
    <div v-if="checkshow">
      <el-button type="primary" size="small" v-for="(item,key) in CardData.data" :key="key" @click="check(item.weldingcontactId)">工艺卡{{key+1}}</el-button>
    </div>
    <div v-if="heatshow">
      <el-button type="primary" size="small" v-for="(item,key) in CardData.data" :key="key" @click="mode(item.heatId,item.selectedTreeNode2)">工艺卡{{key+1}}</el-button>
    </div>
    <div v-if="machshow">
      <el-button type="primary" size="small" v-for="(item,key) in CardData.data" :key="key" @click="mode(item.machingId,item.selectedTreeNode3)">工艺卡{{key+1}}</el-button>
    </div>
    <span slot="footer" class="dialog-footer">
      <el-button @click="dialogVisible = false">关 闭</el-button>
    </span>
    </el-dialog>

    <!-- 焊接信息 -->
    <welding-dialog ref="weldcomponent"></welding-dialog>

    <!-- 制造工艺 -->
    <craftsmanship-dialog ref="cratsmanshipcomponent"></craftsmanship-dialog>

    <!-- 热处理信息 -->
    <heattreatment-dialog ref="heattreatmentcomponent"></heattreatment-dialog>

    <!-- 机械加工工艺 -->
    <machining-dialog ref="machcomponent"></machining-dialog>
  </div>
</template>

<script>
import { VueGoodTable } from "vue-good-table";
import axios from "axios";
import { Loading, Form } from 'element-ui';
import ExamineDialog from './Dialog';
import WeldingDialog from "../../basicdata/components/WeldingDialog.vue"
import CraftsmanshipDialog from "../../basicdata/components/CraftsmanshipDialog.vue"
import HeattreatmentDialog from "../../basicdata/components/HeattreatmentDialog.vue"
import MachiningDialog from "../../basicdata/components/MachiningDialog.vue"

export default {
  name: 'ExamineTable',
  props: {
    item: String,
    selectvalue: String,
  },
  components: {
    VueGoodTable,
    ExamineDialog,
    WeldingDialog,
    CraftsmanshipDialog,
    HeattreatmentDialog,
    MachiningDialog
  },
  // inject:["reload"],
  data () {
    return {
      rows: [],
      show_goodtable:true,
      dialog: {
        centerDialogVisible: false,
        number: ''
      },
      show_btn:true,
      show_btn1:true,
      loadingInstance: '',
      dialogVisible:false,
      Cardtitle:'工艺卡',
      CardData:'',
      modeshow:false,
      checkshow:false,
      heatshow:false,
      machshow:false,
    }
  },
  computed: {
    columns () {
      let head = []
      // 判断点击哪个标签页
      this.item == '未检验'
        ? (head = [
            {
              label: '部件编号',
              field: 'number',
              filterable: true,
            },
            {
              label: '所属项目',
              field: 'project',
              filterable: true,
            },
            {
              label: '部件名称',
              field: 'partName',
              filterable: true,
            },
            {
              label: '部件图号',
              field: 'figure_number',
              filterable: true,
            },
            
            {
              label: '关键零部件',
              field: 'radio',
              filterable: true,
            },
            {
              label: '工艺路线',
              field: 'route',
              filterable: true,
            },
            {
              label: '数量',
              field: 'count',
              filterable: true,
            },{
              label: '制造工艺卡',
              field: 'mode',
              filterable: true,
            },{
              label: '焊接工艺卡',
              field: 'check',
              filterable: true,
            },{
              label: '热处理工艺卡',
              field: 'heat',
              filterable: true,
            },{
              label: '加工工艺卡',
              field: 'maching',
              filterable: true,
            },{
              label: '完工附件',
              field: 'finish',
              filterable: true,
            }
          ])
        : (head = [
            {
              label: '部件编号',
              field: 'number',
              filterable: true,
            },
            {
              label: '所属项目',
              field: 'project',
              filterable: true,
            },
            {
              label: '部件名称',
              field: 'partName',
              filterable: true,
            },
            {
              label: '部件图号',
              field: 'figure_number',
              filterable: true,
            },
            
            {
              label: '关键零部件',
              field: 'radio',
              filterable: true,
            },
            {
              label: '工艺路线',
              field: 'route',
              filterable: true,
            },
            {
              label: '数量',
              field: 'count',
              filterable: true,
            },
            {
              label: '检验日期',
              field: 'checkDate',
            },{
              label: '制造工艺卡',
              field: 'mode',
              filterable: true,
            },{
              label: '焊接工艺卡',
              field: 'check',
              filterable: true,
            },{
              label: '热处理工艺卡',
              field: 'heat',
              filterable: true,
            },{
              label: '加工工艺卡',
              field: 'maching',
              filterable: true,
            },{
              label: '检验详情',
              field: 'result',
            },
            // {
            //   label: '流转单打印',
            //   field: 'checkSituation',
            //   filterable: true,
            // }
          ]);

      return head;
    },

    // 加载框配置项
    options () {
      let obj = {
        background: "rgba(f,f,f,0.5)",
        text: '加载中',
        spinner: 'el-icon-loading',
      };
      return obj;
    }
  },
  methods: {
    // 打印流转单
    print() {
      let routeData = this.$router.resolve({
        name: "Circulation"
      });
      window.open(routeData.href, '_blank');
    },
    print1(row) {
      localStorage.removeItem("number")
      localStorage.setItem("number",row.number)
      localStorage.setItem("route",row.route)
      let routeData = this.$router.resolve({
        name: "Sending_check"
      });
      window.open(routeData.href, '_blank');
    },
    //制造工艺卡
    mode (contactId,selectedTreeNode) {
      this.dialog.CraftsmanshipDialog = true;
      this.$refs.cratsmanshipcomponent.Handlealter(contactId,selectedTreeNode)
    },


    //焊接工艺卡按钮
    check (e) {
      this.dialog.WeldingDialog = true
      this.$refs.weldcomponent.Handlealter(e)
      // this.dialog.number = e.currentTarget.getAttribute('name');// 部件编码
      
    },


    //热处理工艺卡
    heat (heatId,selectedTreeNode2) {
      this.dialog.HeattreatmentDialog = true;
      this.$refs.heattreatmentcomponent.Handlealter(heatId,selectedTreeNode2)
    },


    //机械加工工艺卡按钮
    mach (machingId,selectedTreeNode3) {
      this.dialog.MachiningDialog = true
      this.$refs.machcomponent.Handlealter(machingId,selectedTreeNode3)
      
    },

    // 成功回调
    success (ret) {
      this.$nextTick(() => { 
        this.loadingInstance.close();
      })
      // console.log(ret.data.data)
      if(ret.data.success == 'success'){
        this.item == '未检验'?
        this.rows = ret.data.data : 
        this.rows = ret.data.data
      }else {
        this.rows = '';
      }
      // localStorage.setItem("data",ret.data.data);
    },

    // 失败回调
    error (e) { 

      this.$nextTick(() => { 
        this.loadingInstance.close();
      });

      console.log(e);
    },
    btn_reload(){
      this.show_goodtable=false
      this.$nextTick(() => (this.show_goodtable = true))
    },
    handleClose(done) {
        this.$confirm('确认关闭？')
          .then(_ => {
            done();
          })
          .catch(_ => {});
      },
      //后台查多张工艺卡
      checkCard(figure_number,pNumber,type){
        var fd = new FormData()
        fd.append("flag","getCard")
        fd.append("figure_number",figure_number)
        fd.append("pNumber",pNumber)
        fd.append("type",type)
        this.modeshow=false
        this.checkshow=false
        this.heatshow=false
        this.machshow=false
        switch (type){
          case 'mode':
            axios.post(`${this.baseURL}/examine.php`,fd).then((res)=>{
              if(res.data.success=='success'){
                this.modeshow = true
                this.CardData = res.data
                this.Cardtitle = "制造工艺卡"
                this.dialogVisible = true
              }else{
                this.$message.error('加载工艺卡错误！');
              }
            })
            break;
          case 'check':
            axios.post(`${this.baseURL}/examine.php`,fd).then((res)=>{
              if(res.data.success=='success'){
                this.checkshow = true
                this.CardData = res.data
                this.Cardtitle = "焊接工艺卡"
                this.dialogVisible = true
              }else{
                this.$message.error('加载工艺卡错误！');
              }
            })
            break;
          case 'heat':
            axios.post(`${this.baseURL}/examine.php`,fd).then((res)=>{
              if(res.data.success=='success'){
                this.heatshow = true
                this.CardData = res.data
                this.Cardtitle = "热处理工艺卡"
                this.dialogVisible = true
              }else{
                this.$message.error('加载工艺卡错误！');
              }
            })
            break;
          case 'mach':
            axios.post(`${this.baseURL}/examine.php`,fd).then((res)=>{
              if(res.data.success=='success'){
                this.machshow = true
                this.CardData = res.data
                this.Cardtitle = "加工工艺卡"
                this.dialogVisible = true
              }else{
                this.$message.error('加载工艺卡错误！');
              }
            })
            break;
          default:
            this.$message.error('加载工艺卡错误！');
        }
      }
  },

  mounted () {
    localStorage.item=this.item
    var state = localStorage.item
    // console.log(state)
    if(state == "未检验"){
      state = 1
    }else if(state =="合格"){
      state = 3
    }else{
      state = 4
    }
    localStorage.selectvalue=this.selectvalue
    var selectvalue=localStorage.selectvalue
    this.loadingInstance = Loading.service(this.options)
    var fd = new FormData()
        // fd.append("flag","Select")
        fd.append("flag","State")
        fd.append("state",state)
        fd.append("selectvalue",selectvalue)
    axios.post(`${this.baseURL}/examine.php`,fd)
    .then(this.success)
    .catch(this.error)
  },
  watch: {

    // 切换页面加载数据
    item: function() {
      this.loadingInstance = Loading.service(this.options)
      localStorage.item=this.item
      var state = localStorage.item
      var selectvalue = localStorage.selectvalue
      // console.log(selectvalue)
      if(state == "未检验"){
        state = 1
      }else if(state =="合格"){
        state = 3
      }else{
        state = 4
      }
      var fd = new FormData()
          fd.append("flag","State")
          fd.append("state",state)
          fd.append("selectvalue",selectvalue)
      axios.post(`${this.baseURL}/examine.php`,fd)
      .then(this.success)
      .catch(this.error)
    },
    selectvalue: function() {
      this.loadingInstance = Loading.service(this.options)
      localStorage.selectvalue=this.selectvalue
      var selectvalue = localStorage.selectvalue
      var state = localStorage.item
      // console.log(state)
      if(state == "未检验"){
        state = 1
      }else if(state =="合格"){
        state = 3
      }else{
        state = 4
      }
      var fd = new FormData()
          fd.append("flag","State")
          fd.append("state",state)
          fd.append("selectvalue",selectvalue)
      axios.post(`${this.baseURL}/examine.php`,fd)
      .then(this.success)
      .catch(this.error)
    },
  },
}
</script>

<style scoped>
  .show_img:link {
    color: blue;
  }
  .btn_reload{
    position: absolute;
    z-index: 5;
    width: 70px;
    right: 12px;
    top: 6px;
  }
</style>
