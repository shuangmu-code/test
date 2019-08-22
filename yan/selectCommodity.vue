<template>
    <!-- <div>
        <el-button type="text" @click="selectCommodityVisible = true">打开嵌套表格的 Dialog</el-button> -->
        <el-dialog title="请选择类目" 
                :visible.sync="selectCommodityVisible" 
                :close-on-click-modal="false"
                width="46%">
            <div style="margin-top:-25px">
                <el-input class="dialog-search-input" v-model="keywords" placeholder="请输入类目名称/税收分类编码进行查询"></el-input>
                <el-button type="primary" class="dialog-search-btn">搜索</el-button>
            </div>
            <el-table ref="multipleTable" :data="tables" tooltip-effect="dark" height="390px"
                     style="margin-top:45px"
                    :header-cell-style="getRowClass"  @select-all="dialogCheck" @select="dialogCheck">
                <el-table-column type="selection" width="50px" ></el-table-column> 
                <el-table-column prop="name" label="类目名称" show-overflow-tooltip></el-table-column>
                <el-table-column prop="taxCode" label="税收分类编码"></el-table-column>
                <el-table-column prop="taxRate" label="税率" width="100px"></el-table-column>
            </el-table>
        </el-dialog>
    <!-- </div>    -->
</template>

<script>
export default {
    data(){
        return{
            selectCommodityVisible: false,
            keywords:'',
            checked:false,
            selectListStart:[],
        }
    },

    computed:{
        //模糊搜索
        tables(){
            const keywords=this.keywords
            if(keywords){
                return this.selectListStart.filter(item=>{
                    return Object.keys(item).some(key=>{
                        return String(item[key]).toLowerCase().indexOf(keywords)>-1
                    })
                })
            }
            return this.selectListStart
        }
    },

    methods:{
        init (){
            this.selectCommodityVisible=true

            this.$http({
                url: this.$http.adornUrl('/manage/category/select'),
                method: 'get',
                params: this.$http.adornParams()
            }).then(({data}) => {
                this.selectListStart = data && data.code === 0 ? data.list : []
            })

        },
        //设置表格第一行的颜色
        getRowClass({ row, column, rowIndex, columnIndex}){
            if(rowIndex==0){
                return 'background:rgba(245,247,250,1)'
            }else{
                return ''
            }
        },
        //单选操作，全选按钮失效操作
        dialogCheck: function(selection, row) {
            this.$refs.multipleTable.clearSelection()
            if (selection.length === 0) { // 判断selection是否有值存在
                return
            }
            if (row) {
                this.selectioned = row
                this.$refs.multipleTable.toggleRowSelection(row, true)

                //$emit来触发一个自定义事件change，并传递一个参数val，val就是子组件要传递给父组件的值
                this.$emit('change',selection) 
                this.$refs.multipleTable.clearSelection()
                this.selectCommodityVisible=false
            }
        },
        //取消单选的checkbox
        dialogCheckChange(row) {
            if (row.length === 0) {
                this.selectioned = null
            }
        }
    }
}
</script>


<style>
  .el-dialog__title{
    margin-left:10px;
 }
  .el-dialog__headerbtn{
    margin-right:10px;
 }
  .el-dialog__body{
    height: 470px;
    margin-left: 10px;
    margin-right: 10px;
  }
  .dialog-search-input .el-input__inner{
    border-radius:4px 0px 0px 4px!important;
 }
  .dialog-search-input{
    width:calc(100% - 80px);
    height:32px;
    float:left;
 }
  .dialog-search-btn{
    width:80px;
    height:36px;
    background:rgba(24,144,255,1);
    border-radius:0px 4px 4px 0px;
    float:left;
 }
 .has-gutter .gutter{
    background: rgba(245, 247, 250, 1);
 }

</style>
