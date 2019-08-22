<template>
<div>
    <el-card> 
        <div class="content_title">查询条件</div>
        <el-form v-model="dataForm" :inline="true"  class="search_box">
             <label class="content_text" >税收分类编码：</label>
             <el-input v-model="dataForm.tax_code" size="mini" class="input_css" placeholder="请输入" ></el-input>

             <label class="content_text" style="position:relative;left:10px;" >货物和劳务名称：</label>
             <el-input v-model="dataForm.name" size="mini" class="input_css" style="position:relative;left:10px;" placeholder="请输入" ></el-input>

             <label class="content_text" style="position:relative;left:30px;"  >简称：</label>
             <el-input v-model="dataForm.class_name" size="mini" class="input_css" style="position:relative;left:30px;"  placeholder="请输入" ></el-input>

             <el-button type="primary" class="content_button" size="mini" style="background:rgba(24,144,255,1);padding:0px;" @click="query">查询</el-button>
             <el-button type="primary" class="content_button" size="mini" style="background:rgba(255,255,255,1);color:rgba(0,0,0,0.65);border:1px solid rgba(217,217,217,1);padding:0px;" @click="reset">重置</el-button>
         
        </el-form>
    </el-card>
         <el-card style="margin-top:16px;">
            <el-table
             :data="dataList"
             v-loading="dataListLoading"
             style = "width : 100%;">
             <el-table-column
               type="index"
               label="序号"
               width="60px">
               </el-table-column>
               <el-table-column
               prop="taxCode"
               label="税收分类编号"
               width="224px">
               </el-table-column>
               <el-table-column
               prop="name"
               label="货物和劳务名称"
               width="160px">
               </el-table-column>
               <el-table-column
               prop="className"
               label="商品和服务分类简称"
               width="160px">
               </el-table-column>
               <el-table-column
               prop="description"
               label="说明"
               width="600px"
               show-overflow-tooltip>
               </el-table-column>
               <el-table-column
               prop="taxRate"
               label="增值税税率">
               </el-table-column>
            </el-table>
              <el-pagination 
               @size-change="sizeChangeHandle"
               @current-change="currentChangeHandle"
               :current-page="pageIndex"
               :page-sizes="[10, 20, 30, 40]"
               :page-size="10"
               :total="totalPage"
               layout="total, sizes, prev, pager, next, jumper">
               </el-pagination>
         </el-card>
    
  </div>
</template>   

<script>
export default {
    data(){
        return{
            dataForm:{
                tax_code:'',
                name:'',
                class_name:'',
            },
            dataList:[],
            dataListLoading: false,
           
            pageIndex: 1,
            totalPage: 1,
            }
        },
        activated(){
            this.getDataList()
        },
    methods:{
        query:function(event){
          this.getDataList()
        },
        reset:function(event){
           this.dataForm.tax_code='';
           this.dataForm.name='';
           this.dataForm.class_name='';

        },

        getDataList(){
           this.dataListLoading = true
           this.$http({
                url: this.$http.adornUrl('/manage/commodity/list'),
                method: 'get',
                params: this.$http.adornParams({
                    'page': this.pageIndex,
                    'limit': this.pageSize,
                    'tax_code': this.dataForm.tax_code, 
                    'name':this.dataForm.name,
                    'class_name':this.dataForm.class_name
                })
          }).then(({data}) => {
              if (data && data.code === 0) {
                this.dataList = data.page.list
                this.totalPage = data.page.totalCount
              } 
              else {
                this.dataList = []
                this.totalPage = 0
              }
              this.dataListLoading = false
          })
         },
            sizeChangeHandle (val) {
              this.pageSize = val
              this.pageIndex = 1
              this.getDataList()
         },
            // 当前页
            currentChangeHandle (val) {
              this.pageIndex = val
              this.getDataList()
         },
    }
}
</script>

<style>
  .search_box {
      margin-left: 14px;
  }
  .content_title{
      font-size:18px;
      font-family:PingFangSC-Regular;
      font-weight:400;
      color:rgba(39,39,39,1);
      line-height:25px;
      margin-left:2%;
  }
  .content_text{
      font-size:14px;
      font-family:PingFangSC-Regular;
      font-weight:400;
      color:rgba(0,0,0,0.85);
      line-height:22px;

  }
  .input_css{
      width:272px;
  }
  .content_button{
      top:66px;
      width:65px;
      height:32px;
      border-radius:4px; 
      margin-left: 40px;  
}
 @media screen and (max-width: 1500px){
  .search_box {
      margin-left: 10px;
  }
  .content_title{
      font-size:13px;
      line-height:18px;
  }
  .content_text{
      width:69px;
      height:16px;
      font-size:10px;
      line-height:16px;
  }
  .input_css{
      width:180px;
  }
 }
 @media screen and (max-width: 1300px){
  .search_box {
      margin-left: 8px;
  
  }
  .content_title{
      font-size:10px;
      line-height:14px;
  }
  .content_text{
      width:55px;
      height:12px;
      font-size:8px;
      line-height:12px;
  }
  .input_css{
      width:150px;
  }
 }
</style>


            






