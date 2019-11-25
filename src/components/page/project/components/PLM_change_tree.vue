<template>
    <div class="main">
        <el-tree
        class="tree"
        :data="data"
        node-key="id"
        default-expand-all
        :render-content="renderContent"
        :expand-on-click-node="false"
        draggable>
        </el-tree> 
        <el-button type="primary" class="button_save" @click="save()">生成树结构json文件</el-button>
        <!-- <el-button type="danger" class="button_cancel" @click="cancel()">取消</el-button>  -->
        <!-- <el-button type="danger" class="button_cancel" @click="reloadtree()">重载树</el-button>  -->
        <el-dialog title="子部件信息" :visible.sync="dialogFormVisible">
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
                <el-button type="primary" >确 定</el-button>
            </div>
        </el-dialog> 
    </div>
</template>
<script>
export default {
    data(){
        return{
            data: [{
                id: 1,
                label: '一级 1',
                children: [{
                    id: 4,
                    label: '二级 1-1',
                    children: [{
                    id: 9,
                    label: '三级 1-1-1'
                    }, {
                    id: 10,
                    label: '三级 1-1-2'
                    }]
                }]
                }, {
                id: 2,
                label: '一级 2',
                children: [{
                    id: 5,
                    label: '二级 2-1'
                }, {
                    id: 6,
                    label: '二级 2-2'
                }]
                }, {
                id: 3,
                label: '一级 3',
                children: [{
                    id: 7,
                    label: '二级 3-1'
                }, {
                    id: 8,
                    label: '二级 3-2',
                    children: [{
                    id: 11,
                    label: '三级 3-2-1'
                    }, {
                    id: 12,
                    label: '三级 3-2-2'
                    }, {
                    id: 13,
                    label: '三级 3-2-3'
                    }]
                }]
            }], 
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
            nodedata:{}
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
                console.log(this.firstdata)
            }  
        },
    },
    methods:{
      renderContent(h, { node, data, store }) {
        return (
          <span>
            <span>{node.label}</span>
            <span>
              <el-button size="mini" type="text" on-click={ () => this.append(data) }>添加子部件</el-button>
              <el-button size="mini" type="text" on-click={ () => this.remove(node, data) }>删除部件</el-button>
            </span>
          </span>);
      },
      
      append(data) {
        // this.nodedata=data
        // this.form={
        //         label: '',
        //         figure_number: '',
        //         material: '',
        //         count: '',
        //         remark: ''
        //     }
        // this.dialogFormVisible=true
        this.$prompt('请输入子部件名称', '添加子部件', {
          confirmButtonText: '确定',
          cancelButtonText: '取消',
        }).then(({ value }) => {
            const newChild = {  label: value, children: [] };
            if (!data.children) {
                this.$set(data, 'children', []);
            }
            data.children.push(newChild);
        }).catch(() => {
          this.$message({
            type: 'info',
            message: '取消添加'
          });       
        });
        

      },
      remove(node, data) {
        const parent = node.parent;
        const children = parent.data.children || parent.data;
        const index = children.findIndex(d => d.id === data.id);
        children.splice(index, 1);
      }, 
      formClick(){
        var data=this.nodedata;
        console.log(data)
        var formdata=this.form;
        const newChild = { id: data.id++, label: 'testtest1', children: [] };
        if (!data.children) {
            this.$set(data, 'children', []);
        }
        data.children.push(newChild);
        console.log(data)
        this.dialogFormVisible=false;
      },
      cancel(){
          this.$emit('listen',false)
      },
      save(){
          alert('已生成json文件')
        //   this.$emit('listen',false)
      },
      reloadtree(){
          location.reload();
      }     
    }
}
</script>
<style scoped>
    .main{
        /* border-style: solid; */
        border-width: 1px;
        border-color:black ;
        width: 90%;
        position: relative;
        left: 5%;
    }
    .tree{
        margin-top: 30px;
        margin-bottom: 30px;
        width: 70%;
    }
    .button_save{
        position:absolute;
        right: 5%;
        top: 0px;
    }
    .button_cancel{
        position:absolute;
        right: 3%;
        top: 10px;
    }
</style>