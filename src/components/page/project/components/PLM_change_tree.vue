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
export default {
    data(){
        return{
            data: [], 
            expandlabel:[],
            firstdata:{},
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
                remark: ''                
            }
        }
    },
    props: {
        treedata: String
    },
    watch: {
        treedata : {
            immediate: true,   //如果不加这个属性，父组件第一次传进来的值监听不到
            handler(val) {
                this.data = val
                this.firstdata=val
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
        console.log(store)
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
        const index = children.findIndex(d => d.id === data.id);
        children.splice(index, 1);
      },
      check(store,data,node){
        this.inputdata.label=data.label;
        this.inputdata.figure_number=data.figure_number;
        this.inputdata.material=data.material;
        this.inputdata.count=data.count;
        this.inputdata.remark=data.remark;
      }, 
      formClick(){
        var data=this.triggerCurrenNodeData;
        const newChild = { "hierarchy": data.hierarchy++, "label": this.form.label, "figure_number": this.form.figure_number, "material": this.form.material,"belong_part": data.figure_number,"count": this.form.count,"remark": this.form.remark,"children": [] };
        if (!data.children) {
            this.$set(data, "children", []);
        }
        data.children.push(newChild);
        this.dialogFormVisible=false;
      },
      cancel(){
          this.$emit('listen',false)
      },
      save(){
          this.$emit('listen',false)
      },
      handleDragEnter(draggingNode, dropNode, ev) {
        // console.log('tree drag enter: ', dropNode.label);
        this.expandlabel=[];
        this.expandlabel.push(dropNode.label);
      },
      handleDragEnd(){
        this.expandlabel=[];
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
        position: relative;
        left: 5%;
        height: 450px;
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
        top:400px;
    }
    .input{
        width: 70%;
    }
    .SingleInputDiv{
        margin-top: 10px;
        text-align:right;
    }
</style>