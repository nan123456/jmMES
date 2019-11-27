<template>
  <div>
    <!-- el-tabs的v-model对应el-tab-pane的name ,即显示对应标签页 -->
    <el-tabs type="border-card" v-model="activeName">
      <el-tab-pane name="first" label="信息汇总">
        <el-collapse v-model="activeNames" @change="handleChange;handleChangeBOX">
          <el-collapse-item title="基本信息" name="1">
            <el-form ref="form"  label-width="80px">
              <el-form-item label="部件名称">
                <el-col :span="10">
                  <el-input v-model="partfile.name"></el-input>
                </el-col>
              </el-form-item>
              <el-form-item label="图号">
                <el-col :span="10">
                  <el-input v-model="partfile.figure_number" :disabled="true"></el-input>
                </el-col>
              </el-form-item>
              <el-form-item label="规格">
                <el-col :span="10">
                  <el-input v-model="partfile.standard"></el-input>
                </el-col>
              </el-form-item>
              <el-form-item label="数量">
                <el-col :span="10">
                  <el-input v-model="partfile.count"></el-input>
                </el-col>
              </el-form-item>
              <el-form-item label="关键部件">
                <el-col :span="10">
                  <el-input v-model="partfile.radio"></el-input>
                </el-col>
              </el-form-item>
              <el-form-item label="子物料">
                <el-col :span="10">
                  <el-input v-model="partfile.child_material" :disabled="true"></el-input>
                </el-col>
              </el-form-item>
              <el-form-item label="子件编码">
                <el-col :span="10">
                  <el-input v-model="partfile.child_number" :disabled="true"></el-input>
                </el-col>
              </el-form-item>
              <el-form-item>
                <el-button type="primary" @click="handleQrcode(partfile.id)">产品标识卡</el-button>
                <el-button type="primary" v-if='showWelding' @click="handleWelding(partfile.figure_number,partfile.pNumber)">焊接工艺卡</el-button>
                <el-button type="primary" v-if="showCrafts" @click="handleCrafts(partfile.figure_number,partfile.pNumber)">机械制造卡</el-button>
                <el-button type="primary" v-if="showHot" @click="handleHeating(partfile.figure_number,partfile.pNumber)">热处理工艺卡</el-button>
                <el-button type="primary" v-if="showMach" @click="handleMaching(partfile.figure_number,partfile.pNumber)">机械加工卡</el-button>
              </el-form-item>
            </el-form>
          </el-collapse-item>
          <el-collapse-item title="车间" name="2">
            <el-table
            :data="tableData"
            style="width: 100%">
            <el-table-column
              prop="route"
              label="车间"
              width="50px">
            </el-table-column>
            <el-table-column
              prop="route_line"
              label="工艺路线">
            </el-table-column>
            <el-table-column
              prop="ctime"
              label="排产时间">
            </el-table-column>
            <el-table-column
              prop="otime"
              label="交付时间">
            </el-table-column>
            <el-table-column
              prop="stime"
              label="就工时间">
            </el-table-column>
            <el-table-column
              prop="utime"
              label="完工时间">
            </el-table-column>
            <el-table-column
              prop="notNum"
              label="不合格次数">
            </el-table-column>
            <el-table-column
              prop="remark"
              label="不合格原因">
            </el-table-column>
            <el-table-column
              prop="backMark"
              label="退产">
            </el-table-column>
            <el-table-column
              prop="reason"
              label="退产原因">
            </el-table-column>
          </el-table>
          </el-collapse-item>
          <el-collapse-item title="附件" name="3">
            <span><b>完工未检验图片：</b></span>
            <div v-for="(tdata,index) in tableData" :key="index">
              <span>{{tdata.route}}路线：</span>
              <div v-for="(photo,i) in finishphoto[index]" :key="i">
                <img v-if='finishitem[index+i]' :src="finishitem[index+i]" class="img"/>
              </div>
            </div>
            <span><b>检验图片：</b></span>
            <div v-for="(tdata,index) in tableData" :key="index">
              <span>{{tdata.route}}路线：</span>
              <div v-for="(photo,i) in inspectphoto[index]" :key="i">
                <img v-if='inspectitem[index+i]' :src="inspectitem[index+i]" class="img"/>
              </div>
            </div>
            <span><b>不合格处理图片：</b></span>
            <div v-for="(tdata,index) in tableData" :key="index">
              <span>{{tdata.route}}路线：</span>
              <div v-for="(photo,i) in unqualifiedphoto[index]" :key="i">
                <img v-if='unqualifieditem[index+i]' :src="unqualifieditem[index+i]" class="img"/>
              </div>
            </div>
            <!-- <img :src="photourl" class="img"/> -->
          </el-collapse-item>
        </el-collapse>
      </el-tab-pane>
      <el-tab-pane name="second" label="子部件进度">
        <p>黑色为正在进行中状态；绿色为已完成状态；灰色为未开工状态</p>
        <part-sch :partschdata="this.partschdata" @change="handleChangeBOX"></part-sch>
      </el-tab-pane>
    </el-tabs>
  </div>
</template>

<script>
import axios from 'axios'
import PartSch from '../../workshop/components/PartSch'
export default {
  name: 'Partfile',
  components: {
    PartSch
  },
  props: {
    lxid: String
  },
  data () {
    return {
      partschdata: {},
      activeNames: ['1'],
      partfile:[],
      mylxid: '',
      finishitem:{},
      inspectitem:{},
      unqualifieditem:{},
      photo:[],//按车间顺序照片个数
      finishphoto:[],
      inspectphoto:[],
      unqualifiedphoto:[],
      activeName: 'first',
      tableData: [],
      photourl:'',
      showWelding:true,
      showCrafts:true,
      showHot:true,
      showMach:true
    }
  },// 监听数据的变化
  watch: {
    lxid : {
      immediate: true,   //如果不加这个属性，父组件第一次传进来的值监听不到
      handler(val) {
        this.mylxid = val;
        this.activeName = 'first'
      }  
    },
    // 声明mylxid的原因:vue不允许直接修改props的值，通过声明赋值新变量重新渲染组件
    mylxid: {
      immediate: true,   //如果不加这个属性，父组件第一次传进来的值监听不到
      handler(val) {
        var file = new FormData() //定义获取partschdata的传值
        file.append('id',val)
        file.append('flag','partfile')
        axios.post(`${this.baseURL}/system/partfile.php`,file).then(this.getPartfiledataSucc)

        var sch = new FormData() //定义获取partschdata的传值
        sch.append('id',val)
        sch.append('flag','partsch')
        axios.post(`${this.baseURL}/part.php`,sch).then(this.getPartschdataSucc)
      
        var fd = new FormData() //定义获取partschdata的传值
        fd.append('id',val)
        fd.append('flag','partdata')
        axios.post(`${this.baseURL}/system/partfile.php`,fd).then(this.getpartdataSucc)
      }  
    }
  },
  methods: {
    getPartfiledataSucc(res) {
      console.log(res)
      this.tableData = []
      this.finishitem = []
      this.inspectitem = []
      this.unqualifieditem = []
      if(res.data.success =='success'){
        this.tableData = res.data.data
      }
      this.tableData.forEach(element => {
        this.finishphoto.push(element.finishurl.length)
        for(let i = 0;i<element.finishurl.length;i++){
          this.finishitem.push(element.finishurl[i])
        }
        this.inspectphoto.push(element.inspecturl.length)
        for(let i = 0;i<element.inspecturl.length;i++){
          this.inspectitem.push(element.inspecturl[i])
        }
        this.unqualifiedphoto.push(element.unqualifiedurl.length)
        for(let i = 0;i<element.unqualifiedurl.length;i++){
          this.unqualifieditem.push(element.unqualifiedurl[i])
        }
      });
      // this.photourl=res.data.data[0].photourl;
      // console.log(this.tableData)
      // console.log(this.item)
    },
    // 产品标识卡
    handleQrcode(id) {
      window.open("./#/qrcode?id="+id+"&modid="+this.partfile.modid)
    },
    getpartdataSucc(res) {
      this.partfile = []
      if(res.data.success =='success'){
        this.partfile = res.data.data
      }
      //判断是否存在焊接工艺卡
      var fd = new FormData()
        fd.append("flag","welding")
        fd.append("figure_number",this.partfile.figure_number)
        fd.append("pnumber",this.partfile.pNumber)
        axios.post(`${this.baseURL}/craft/part_card.php`,fd).then((res)=>{
          console.log(res)
          if(res.data.success=='success'){
            this.showWelding = true
          }else {
            this.showWelding = false
          }
        })
        //判断是否存在机械工艺卡
        var fd1 = new FormData()
        fd1.append("flag","crafts")
        fd1.append("figure_number",this.partfile.figure_number)
        fd1.append("pnumber",this.partfile.pNumber)
        axios.post(`${this.baseURL}/craft/part_card.php`,fd1).then((res)=>{
          if(res.data.success=='success'){
            this.showCrafts = true
          }else {
            this.showCrafts = false
          }
        })
        //热处理
        var fd2 = new FormData()
        fd2.append("flag","heating")
        fd2.append("figure_number",this.partfile.figure_number)
        fd2.append("pnumber",this.partfile.pNumber)
        axios.post(`${this.baseURL}/craft/part_card.php`,fd2).then((res)=>{
          if(res.data.success=='success'){
            this.showHot = true
          }else {
            this.showHot = false
          }
        })
        //机加工
        var fd3 = new FormData()
        fd3.append("flag","maching")
        fd3.append("figure_number",this.partfile.figure_number)
        fd3.append("pnumber",this.partfile.pNumber)
        axios.post(`${this.baseURL}/craft/part_card.php`,fd3).then((res)=>{
          if(res.data.success=='success'){
            this.showMach = true
          }else {
            this.showMach = false
          }
        })
    },
    // 焊接工艺卡
    handleWelding(figure_number,pnumber) {
      // console.log(figure_number)
      var fd = new FormData()
      fd.append("flag","welding")
      fd.append("figure_number",figure_number)
      fd.append("pnumber",pnumber)
      axios.post(`${this.baseURL}/craft/part_card.php`,fd).then((res)=>{
        if(res.data.success=='success'){
          // console.log(res.data.id)
          window.open("./#/Weldingprinter?contactId="+res.data.id)
        }else {
          alert("该部件还未上传焊接工艺卡！")
        }
      })
    },
    //机械制造卡
    handleCrafts(figure_number,pnumber) {
      // console.log(figure_number)
      var fd = new FormData()
      fd.append("flag","crafts")
      fd.append("figure_number",figure_number)
      fd.append("pnumber",pnumber)
      axios.post(`${this.baseURL}/craft/part_card.php`,fd).then((res)=>{
        if(res.data.success=='success'){
          // console.log(res.data.id)
          window.open("./#/Craftsmanshipprinter?contactId="+res.data.id)
        }else {
          alert("该部件还未上传机械制造卡！")
        }
      })
    },
    // 热处理工艺卡
    handleHeating(figure_number,pnumber) {
      // console.log(figure_number)
      var fd = new FormData()
      fd.append("flag","heating")
      fd.append("figure_number",figure_number)
      fd.append("pnumber",pnumber)
      axios.post(`${this.baseURL}/craft/part_card.php`,fd).then((res)=>{
        if(res.data.success=='success'){
          // console.log(res.data.id)
          window.open("./#/Heattreatmentprinter?contactId="+res.data.id)
        }else {
          alert("该部件还未上传热处理工艺卡！")
        }
      })
    },
    // 机械加工工艺卡
    handleMaching(figure_number,pnumber) {
      // console.log(figure_number)
      var fd = new FormData()
      fd.append("flag","maching")
      fd.append("figure_number",figure_number)
      fd.append("pnumber",pnumber)
      axios.post(`${this.baseURL}/craft/part_card.php`,fd).then((res)=>{
        if(res.data.success=='success'){
          // console.log(res.data.id)
          window.open("./#/machiningprinter?contactId="+res.data.id)
        }else {
          alert("该部件还未上传机械加工卡！")
        }
      })
    },
    //折叠面板
    handleChange(val) {
      // console.log(val);
    },
    handleChangeBOX(id,btn_state) {
      // console.log(id)
      this.$emit("change",btn_state)
      this.mylxid = ''
      this.mylxid = id
      this.activeName = 'first'
      // console.log(this.lxid)
    },
    getPartschdataSucc(res) {
      // console.log(res.data)
      this.partschdata = {"item":[]}
      if(res.data.success) {
        this.partschdata = res.data
      }else {
        this.partschdata = {"key":"default"}
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
</style> 