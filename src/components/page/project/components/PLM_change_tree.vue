<template>
    <div class="main">
        <el-tree
        class="tree"
        :data="data"
        node-key="label"
        @node-drag-end="handleDragEnd"
        @node-drag-enter="handleDragEnter"
        :default-expanded-keys="expandlabel"
        :render-content="renderContent"
        :expand-on-click-node="false"
        draggable>
        </el-tree> 
        <el-button type="primary" class="button_save" @click="save()">保存</el-button>
        <el-button type="danger" class="button_cancel" @click="cancel()">取消</el-button> 
        <div class="AllInputDiv">
            <div class="SingleInputDiv"><label>部件名称:</label><el-input v-model="inputdata.label" readonly="true" class="input"></el-input></div>
            <div class="SingleInputDiv"><label>部件图号:</label><el-input v-model="inputdata.figure_number" readonly="true" class="input"></el-input></div>
            <div class="SingleInputDiv"><label>材料规格:</label><el-input v-model="inputdata.material" readonly="true" class="input"></el-input></div>
            <!-- <div class="SingleInputDiv"><label>部件层级:</label><el-input v-model="inputdata.hierarchy" readonly="true" class="input"></el-input></div> -->
            <div class="SingleInputDiv"><label>数量:</label><el-input v-model="inputdata.count" readonly="true" class="input"></el-input></div>
            <div class="SingleInputDiv"><label>备注:</label><el-input v-model="inputdata.remark" readonly="true" class="input"></el-input></div>
        </div>
        <el-dialog title="子部件信息" :visible.sync="dialogFormVisible" :modal="false">
            <el-form :model="form">
                <el-form-item label="子部件名称" :label-width="formLabelWidth">
                    <el-input v-model="form.label" placeholder="例如：飓风飞椅挡圈"></el-input>
                </el-form-item>
                <el-form-item label="子部件图号" :label-width="formLabelWidth">
                    <el-input v-model="form.figure_number" placeholder="例如：FY-48B.01.01-02"></el-input>
                </el-form-item>
                <el-form-item label="子部件规格材料" :label-width="formLabelWidth">
                    <el-input v-model="form.material" placeholder="例如：Q235-B"></el-input>
                </el-form-item>
                <el-form-item label="子部件数量" :label-width="formLabelWidth">
                    <el-input v-model="form.count" placeholder="例如：8"></el-input>
                </el-form-item>
                <el-form-item label="子部件备注" :label-width="formLabelWidth">
                    <el-input v-model="form.remark"></el-input>
                </el-form-item>
            </el-form>
            <div slot="footer" class="dialog-footer">
                <el-button @click="dialogFormVisible = false">取 消</el-button>
                <el-button type="primary" @click="formClick()">确 定</el-button>
            </div>
        </el-dialog> 
    </div>
</template>
<script>
import axios from 'axios'
export default {
    data(){
        return{
            data: [], 
            expandlabel:[],
            firstdata:{},
            product_id:'',
            defaultProps: {
                children: 'children',
                label: 'label'
            },
            form: {
                label: '',
                figure_number: '',
                material: '',
                count: '',
                remark: ''
            },
            dialogFormVisible:false,
            formLabelWidth:"120px",
            triggerCurrenNodeData :{},
            triggerCurrenNode:{},
            inputdata:{
                label: '',
                figure_number: '',
                material: '',
                count: '',
                remark: '',
                hierarchy:''                
            },
            operating_data:[]
        }
    },
    props: {
        treedata: String,
        name: String
    },
    watch: {
        treedata : {
            immediate: true,   //如果不加这个属性，父组件第一次传进来的值监听不到
            handler(val) {
                this.data=val
            }  
        },
        name : {
            immediate: true,   //如果不加这个属性，父组件第一次传进来的值监听不到
            handler(val) {
                this.product_id = val
            }  
        },
    },
    methods:{
      renderContent(h, { node, data, store }) {
        return (
          <span>
            <span>{node.label}</span>
            <span>
              <el-button size="mini" type="text" on-click={ () => this.append(store,data,node) }>添加子部件</el-button>
              <el-button size="mini" type="text" on-click={ () => this.check(store,data,node) }>查看部件信息</el-button>
              <el-button size="mini" type="text" on-click={ () => this.remove(node,data) }>删除部件</el-button>
            </span>
          </span>);
      },
      
      append(store,data,node) {
        this.triggerCurrenNodeData=data
        this.triggerCurrenNode=node
        // console.log(store)
        this.form={
                label: '',
                figure_number: '',
                material: '',
                count: '',
                remark: ''
            }
        this.dialogFormVisible=true
      },
      remove(node, data) {
        const parent = node.parent;
        const children = parent.data.children || parent.data;
        const index = children.findIndex(d => d.label === data.label);
        children.splice(index, 1);
        var operating='删除了“'+parent.data.label+'“的子部件”'+data.label+'“';
        this.operating_data.push(operating)
        // console.log(this.operating_data)     
      },
      check(store,data,node){
        this.inputdata.label=data.label;
        this.inputdata.figure_number=data.figure_number;
        this.inputdata.material=data.material;
        this.inputdata.count=data.count;
        this.inputdata.remark=data.remark;
        this.inputdata.hierarchy=data.hierarchy;
      }, 
      formClick(){
        var data=this.triggerCurrenNodeData;
        const newChild = { "hierarchy": data.hierarchy++, "label": this.form.label, "figure_number": this.form.figure_number, "material": this.form.material,"belong_part": data.figure_number,"count": this.form.count,"remark": this.form.remark,"children": [] };
        if (!data.children) {
            this.$set(data, "children", []);
        }
        data.children.push(newChild);
        var operating='“'+data.label+'“增加了子部件”'+this.form.label+'“';
        this.operating_data.push(operating)
        // console.log(this.operating_data)         
        this.dialogFormVisible=false;
      },
      cancel(){
          this.$emit('listen',false)
      },
      save(){
            this.$prompt('请输入BOM生成树的名称', '提示', {
            confirmButtonText: '确定',
            cancelButtonText: '取消',
            inputPlaceholder:'输入内容不得超过40个字，BOM树名称不能重复',
            }).then(({value}) => {
                if(value.length>40){
                    this.$message({
                        type: 'error',
                        message: '超过最大限制字数'
                    }); 
                }else{
                    this.saveTree(value);
                }
            }).catch(() => {
                this.$message({
                    type: 'info',
                    message: '已取消保存'
                });          
            });          

      },
      handleDragEnter(draggingNode, dropNode, ev) {
        // console.log('tree drag enter: ', dropNode.label);
        this.expandlabel=[];
        this.expandlabel.push(dropNode.label);
      },
      handleDragEnd(draggingNode, dropNode, dropType, ev){
        //   console.log(draggingNode)
        //   if(dropType=='inner'){
        //     var fatherHierarchy=parseInt(dropNode.data.hierarchy);
        //     if(dropNode.data.hierarchy==undefined){
        //         fatherHierarchy=0;
        //     }
        //     draggingNode.data.hierarchy=fatherHierarchy+1;              
        //   }else if(dropType=='after'||dropType=='before'){
        //     var fatherHierarchy=parseInt(dropNode.data.hierarchy);
        //     draggingNode.data.hierarchy=fatherHierarchy;
        //   }

        //   draggingNode.data.hierarchy=draggingNode.level;

        if(dropType=='after'){
            var type='兄弟部件'
            var operating='“'+draggingNode.data.label+'“经过移动变成了”'+dropNode.data.label+'“的'+type;
            this.operating_data.push(operating)
        }else if(dropType=='before'){
            var type='兄弟部件'
            var operating='“'+draggingNode.data.label+'“经过移动变成了”'+dropNode.data.label+'“的'+type;
            this.operating_data.push(operating)
        }else if(dropType=='inner'){
            var type='子部件'
            var operating='“'+draggingNode.data.label+'“经过移动变成了”'+dropNode.data.label+'“的'+type;
            this.operating_data.push(operating)
        }
        this.expandlabel=[];
      },
      saveTree(value){
        const that=this;
        var fd = new FormData()
        var useraccount=localStorage.account;
        const tree_json = JSON.stringify(this.data);
        fd.append("flag","savePlmJson")
        fd.append("product_id",this.product_id)
        fd.append("tree_json",tree_json)
        fd.append("tree_name",value)
        fd.append("create_user_account",useraccount)
        axios.post(`${this.baseURL}/tree.php`,fd).then(function (res){
            if(res.data.success=='success'){
                const treeid=res.data.data.treeid;
                that.saveOperating(value,treeid);
                // that.$message({
                //     type: 'success',
                //     message: '保存成功'
                // });  
                // that.$emit('listen',false)
            }else if(res.data.success=='chongfu'){
                that.$message({
                    type: 'error',
                    message: '有重复的BOM树名称'
                }); 
            }else{
                that.$message({
                    type: 'error',
                    message: '保存失败'
                }); 
            }
        })    
      },
      saveOperating(treename,treeid){
        var firstOperatData='创建了'+treename;
        this.operating_data.unshift(firstOperatData)
        // console.log(treename)
        // console.log(treeid)
        // console.log(this.product_id)
        // console.log(localStorage.username)
        const that=this;
        var fd = new FormData()
        fd.append("flag","savePlmOperatingData")
        fd.append("treename",treename)
        fd.append("treeid",treeid)
        fd.append("product_id",this.product_id)
        fd.append("username",localStorage.username)
        fd.append("content",JSON.stringify(this.operating_data))
        axios.post(`${this.baseURL}/tree.php`,fd).then(function (res){
            if(res.data.success=='success'){
                that.operating_data=[];
                that.$message({
                    type: 'success',
                    message: '保存成功'
                });  
                that.$emit('listen',false)
            }
        })         

      }        
    }
}
</script>
<style scoped>
    .main{
        border-style: solid;
        border-width: 1px;
        border-color:rgba(0,0,0,0.5) ;
        width: 90%;
        position: absolute;
        top: 15px;
        left: 5%;
        height: 400px;
        background-color: white;
        
    }
    .tree{
        margin-top: 30px;
        margin-bottom: 30px;
        width: 60%;
        overflow: scroll;
    }
    .button_save{
        position:absolute;
        right: 5%;
        top: 10px;
    }
    .button_cancel{
        position:absolute;
        right: 5%;
        top: 50px;
    }
    .AllInputDiv{
        width: 30%;
        position:fixed;
        right: 10%;
        top:300px;
    }
    .input{
        width: 70%;
    }
    .SingleInputDiv{
        margin-top: 10px;
        text-align:right;
    }
</style>