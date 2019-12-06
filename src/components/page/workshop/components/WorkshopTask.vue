<template>
  <div>
    <el-row :gutter="12">
      <el-col>
        <div style="height:40px;width: 100%;"></div>
      </el-col>
    </el-row>
    <el-row :gutter="12">
      <el-col :span="4">
        <el-card shadow="hover" class="box" @click.native="WorkshopType('http://jinmabrand.work/jmlook/#/?workshop=K')">
          <h3 class="TxtTitle">K开料车间</h3>
          <p>车间任务：{{count[0]}}条</p>
        </el-card>
      </el-col>
      <el-col :span="4" :push="1">
        <el-card shadow="hover" class="box" @click.native="WorkshopType('http://jinmabrand.work/jmlook/#/?workshop=TK')">
          <h3 class="TxtTitle">TK开料车间</h3>
          <p>车间任务：{{count[1]}}条</p>
        </el-card>
      </el-col>
      <el-col :span="4" :push="2">
        <el-card shadow="hover" class="box" @click.native="WorkshopType('http://jinmabrand.work/jmlook/#/?workshop=S')">
          <h3 class="TxtTitle">安装S</h3>
          <p>车间任务：{{count[2]}}条</p>
        </el-card>
      </el-col>
      <el-col :span="4" :push="3">
        <el-card shadow="hover" class="box" @click.native="WorkshopType('http://jinmabrand.work/jmlook/#/?workshop=F')">
          <h3 class="TxtTitle">玻璃钢F</h3>
          <p>车间任务：{{count[3]}}条</p>
        </el-card>
      </el-col>
      <el-col :span="4" :push="4">
        <el-card shadow="hover" class="box" @click.native="WorkshopType('http://jinmabrand.work/jmlook/#/?workshop=G')">
          <h3 class="TxtTitle">电气G</h3>
          <p>车间任务：{{count[4]}}条</p>
        </el-card>
      </el-col>
    </el-row>
    <el-row :gutter="12">
      <el-col>
        <div style="height:40px;width: 100%;"></div>
      </el-col>
    </el-row>
    <el-row :gutter="12">
      <el-col :span="4">
        <el-card shadow="hover" class="box" @click.native="WorkshopType('http://jinmabrand.work/jmlook/#/?workshop=T')">
          <h3 class="TxtTitle">机加T</h3>
          <p>车间任务：{{count[5]}}条</p>
        </el-card>
      </el-col>
      <el-col :span="4" :push="1">
        <el-card shadow="hover" class="box" @click.native="WorkshopType('http://jinmabrand.work/jmlook/#/?workshop=I')">
          <h3 class="TxtTitle">机械车间</h3>
          <p>车间任务：{{count[6]}}条</p>
        </el-card>
      </el-col>
      <el-col :span="4" :push="2">
        <el-card shadow="hover" class="box" @click.native="WorkshopType('http://jinmabrand.work/jmlook/#/?workshop=L')">
          <h3 class="TxtTitle">结构L</h3>
          <p>车间任务：{{count[7]}}条</p>
        </el-card>
      </el-col>
      <el-col :span="4" :push="3">
        <el-card shadow="hover" class="box" @click.native="WorkshopType('http://jinmabrand.work/jmlook/#/?workshop=J')">
          <h3 class="TxtTitle">探伤</h3>
          <p>车间任务：{{count[8]}}条</p>
        </el-card>
      </el-col>
      <el-col :span="4" :push="4">
        <el-card shadow="hover" class="box" @click.native="WorkshopType('http://jinmabrand.work/jmlook/#/?workshop=W')">
          <h3 class="TxtTitle">外协W</h3>
          <p>车间任务：{{count[9]}}条</p>
        </el-card>
      </el-col>
    </el-row>
  </div>
</template>

<script>
import axios from 'axios'
export default {
  name: "Workshoptask",
  components: {
  },
  data() {
    return {
      count:[]
      // columns: [
      //   {
      //     label: "车间",
      //     field: "Serial",
      //     width:'180px'
      //   },
      //   {
      //     label: "网站",
      //     field: "url"
      //   }
      // ],
      // rows: [{
      //   Serial:"K开料车间",
      //   url:"http://jinmabrand.work/jmlook/#/?workshop=K"
      // },{
      //   Serial:"TK开料车间",
      //   url:"http://jinmabrand.work/jmlook/#/?workshop=TK"
      // },{
      //   Serial:"安装S",
      //   url:"http://jinmabrand.work/jmlook/#/?workshop=S"
      // },{
      //   Serial:"玻璃钢F",
      //   url:"http://jinmabrand.work/jmlook/#/?workshop=F"
      // },{
      //   Serial:"电气G",
      //   url:"http://jinmabrand.work/jmlook/#/?workshop=G"
      // },{
      //   Serial:"机加T",
      //   url:"http://jinmabrand.work/jmlook/#/?workshop=T"
      // },{
      //   Serial:"机械车间",
      //   url:"http://jinmabrand.work/jmlook/#/?workshop=I"
      // },{
      //   Serial:"结构L",
      //   url:"http://jinmabrand.work/jmlook/#/?workshop=L"
      // },{
      //   Serial:"探伤",
      //   url:"http://jinmabrand.work/jmlook/#/?workshop=J"
      // },{
      //   Serial:"外协W",
      //   url:"http://jinmabrand.work/jmlook/#/?workshop=W"
      // }]
    };
  },
  created(){
    this.getdata()
  },
  methods:{
    getdata(){
      axios.post(`${this.baseURL}/Electronic.php`).then((res)=>{
        this.count = res.data
      })
    },
    WorkshopType(url){
      window.open(url)
    }
  }
};
</script>

<style scoped>
  .btn{
    background-color:cornflowerblue;
    color: white
  }
  .box{
    width: 185px;
    height: 185px;
    display: flex;
    align-items: center;
    justify-content: center;
    flex-wrap: wrap;
    text-align: center;
    color: white;
    box-shadow:0px 0px 5px 5px rgba(0, 0, 0, 0.15);
    background:linear-gradient(0deg,rgba(35,134,255,1),rgba(44,178,252,1));
  };
  .TxtTitle{
    text-align: center;
  }

</style>