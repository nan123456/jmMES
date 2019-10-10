<template>
  <div>
    <vue-good-table
      :columns="columns" 
      :rows="rows"
      :lineNumbers="true"
    >
    <template slot="table-row" slot-scope="props">
      <span v-if="props.column.field == 'url'">
        <a target="_blank" :href="props.row.url">{{props.row.url}}</a>
      </span>
      <span v-else>
        {{props.formattedRow[props.column.field]}}
      </span>
    </template>
    </vue-good-table>
  </div>
</template>

<script>
import { VueGoodTable } from "vue-good-table";
import axios from 'axios'
export default {
  name: "Workshoptask",
  components: {
    VueGoodTable
  },
  data() {
    return {
      columns: [
        {
          label: "车间",
          field: "Serial",
          width:'180px'
        },
        {
          label: "网站",
          field: "url"
        }
      ],
      rows: [{
        Serial:"K开料车间",
        url:"http://47.106.161.130:8081/jmlook/#/?workshop=K"
      },{
        Serial:"TK开料车间",
        url:"http://47.106.161.130:8081/jmlook/#/?workshop=TK"
      },{
        Serial:"安装S",
        url:"http://47.106.161.130:8081/jmlook/#/?workshop=S"
      },{
        Serial:"玻璃钢F",
        url:"http://47.106.161.130:8081/jmlook/#/?workshop=F"
      },{
        Serial:"电气G",
        url:"http://47.106.161.130:8081/jmlook/#/?workshop=G"
      },{
        Serial:"机加T",
        url:"http://47.106.161.130:8081/jmlook/#/?workshop=T"
      },{
        Serial:"机械车间",
        url:"http://47.106.161.130:8081/jmlook/#/?workshop=I"
      },{
        Serial:"结构L",
        url:"http://47.106.161.130:8081/jmlook/#/?workshop=L"
      },{
        Serial:"探伤",
        url:"http://47.106.161.130:8081/jmlook/#/?workshop=J"
      },{
        Serial:"外协W",
        url:"http://47.106.161.130:8081/jmlook/#/?workshop=W"
      }]
    };
  },
  created(){
  },
  methods:{
    Handlealter(url){
      alert(url);
    }
  }
};
</script>

<style lang="">
.btn{
  background-color:cornflowerblue;
  color: white
}
</style>