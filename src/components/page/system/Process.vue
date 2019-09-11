<template>
  <div class="table">
        <!-- <div class="crumbs">
            <el-breadcrumb separator="/">
                <el-breadcrumb-item><i class="el-icon-tickets"></i> 电子看板</el-breadcrumb-item>
            </el-breadcrumb>
        </div> -->
        <div class="container">
        	<el-tabs v-model="activeName2" type="card">
				    <el-tab-pane label="让步接收" name="first"><Concession></Concession></el-tab-pane>
				    <el-tab-pane label="产品调单" name="second"><Shiftrecord></Shiftrecord></el-tab-pane>
				  </el-tabs>
        </div>
        <!--<div class="container" id="Workshop">
          <table-list></table-list>
        </div>-->
    </div>
</template>

<script>
// import TableList from './components/ElectronicList'
import Concession from './components/Concession'
import Shiftrecord from './components/Shiftrecord'
export default {
  name: 'Electronic',
  components: {
    // TableList,
    Concession,
    Shiftrecord
  },
  data() {
      return {
        activeName2: 'first'
      };
   }
}
</script>
<style scoped>
  
</style>