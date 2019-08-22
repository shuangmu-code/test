<template>
 <div>
    <el-card>
        <div class="content_title">查询条件</div>
        <el-form v-model="dataForm" :inline="true"  class="search_box" size="mini">
             <span class="content_text" >类目名称：</span>
             <el-input v-model="dataForm.shop_name" class="input_css" placeholder="请输入" ></el-input>


             <span class="content_text">创建时间：</span>
             <el-date-picker v-model="dataForm.shop_create_time" class="input_css" type="daterange" range-separator="-" start-placeholder="开始日期" end-placeholder="结束日期"></el-date-picker>

             <el-button type="primary" @click="query">查询</el-button>
             <el-button  @click="reset">重置</el-button>
         
      </el-form>
    </el-card>

      <el-card style="margin-top:20px">
        <div class="btn_group">
          <el-button type="primary" @click="addOrUpdate(-1)">+新增</el-button>
          <el-button @click="deleteHandle()" :disabled="dataListSelections.length <= 0">删除</el-button>
        </div>
        <el-table
        :data="dataList"
        border
        cell-class-name="category_cell"
        v-loading="dataListLoading"
        @selection-change="selectionChangeHandle"
        class="table">
        <el-table-column
          type="selection"
          header-align="center"
          align="center"
          width="30px">
        </el-table-column>
        <el-table-column
          type="index"
          header-align="center"
          align="center"
          width="50px"
          label="序号">
        </el-table-column>
        <el-table-column
          prop="categoryName"
          header-align="center"
          align="center"
          width="150px"
          label="类目名称"
          show-overflow-tooltip>
        </el-table-column>
        <el-table-column
          prop="createTime"
          header-align="center"
          align="center"
          label="创建时间"
          show-overflow-tooltip>
        </el-table-column>
        <el-table-column
          prop="companyName"
          header-align="center"
          align="center"
          label="销方名称"
          show-overflow-tooltip>
        </el-table-column>
        <el-table-column
          prop="taxNumber"
          header-align="center"
          align="center"
          label="销方税号"
          show-overflow-tooltip>
        </el-table-column>
        <el-table-column
          prop="address"
          header-align="center"
          align="center"
          width="250px"
          label="销方地址"
          show-overflow-tooltip>
        </el-table-column>
        <el-table-column
          prop="phone"
          header-align="center"
          align="center"
          label="销方电话"
          show-overflow-tooltip>
        </el-table-column>
        <el-table-column
          prop="bankAccount"
          header-align="center"
          align="center"
          width="300px"
          label="销方银行账号">
        </el-table-column>
        <el-table-column
         fixed="right"
         header-align="center"
         align="center"
         width="150"
         label="操作">
         <template slot-scope="scope">
          <el-button  type="text" size="small" @click="addOrUpdate(scope.row.categoryId)">编辑</el-button>
          <el-button  type="text" size="small" @click="deleteHandle(scope.row.categoryId)">删除</el-button>
         </template>
      </el-table-column>
     </el-table>
     <el-pagination 
       @size-change="sizeChangeHandle"
       @current-change="currentChangeHandle"
       :current-page="pageIndex"
       :page-sizes="[5, 10, 15, 20]"
       :page-size="pageSize"
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
                shop_name:'',
                shop_number:'',
                shop_create_time:'',
            },
            dataList:[],
            dataListSelections: [],
            dataListLoading: false,
           
            pageIndex: 1,
            pageSize: 5,
            totalPage: 1,
        }
    },
    activated () {
      this.getDataList()
    },
    methods:{
        query:function(event){
          this.getDataList()
        },
        reset:function(event){
           this.dataForm.shop_name='';
           this.dataForm.shop_create_time='';
        },
        addOrUpdate:function(id){
          this.$router.push({
            name: 'category_add',
            params: {
              id: id
            }
          })
        },

        getDataList(){
           this.dataListLoading = true
           
           var t_start = this.dataForm.shop_create_time[0] != undefined ? this.dataForm.shop_create_time[0].toLocaleDateString() : undefined
           var t_end = this.dataForm.shop_create_time[1] != undefined ? this.dataForm.shop_create_time[1].toLocaleDateString() : undefined
           this.$http({
                url: this.$http.adornUrl('/manage/category/list'),
                method: 'get',
                params: this.$http.adornParams({
                    'page': this.pageIndex,
                    'limit': this.pageSize,
                    'name': this.dataForm.shop_name, 
                    'timeStart':t_start,
                    'timeEnd':t_end
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
      deleteHandle(id){
        var ids = id ? [id] : this.dataListSelections.map(item => {
          return item.categoryId
        })
        this.$confirm(`确定对序号=${ids.join(',')}进行${id ? '删除' : '批量删除'}操作吗?`, '删除信息', {
          confirmButtonText: '确定',
          cancelButtonText: '取消',
          type: 'warning'
        }).then(() => {
          this.$http({
            url: this.$http.adornUrl('/manage/category/delete'),
            method: 'post',
            data: this.$http.adornData(ids, false)
          }).then(({data}) => {
            if (data && data.code === 0) {
              this.$message({
                message: '操作成功',
                type: 'success',
                duration: 1500,
                onClose: () => {
                  this.getDataList()
                }
              })
            } else {
              this.$message.error(data.msg)
            }
          })
        }).catch(() => {})
      },
      selectionChangeHandle (val) {
        this.dataListSelections = val
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


<style lang="scss">
.category_cell{
  padding: 0;
}
.search_box {
  margin-left: 28px;
  margin-bottom: 8px;
}
.content_title{
    font-size:15px;  
  }
.content_text{
    font-size:14px;
    font-family:PingFangSC-Regular;
    font-weight:400;
    color:rgba(0,0,0,0.85);
    line-height:22px;
}
.input_css{
    width:250px;
    height:32px;
    background:rgba(255,255,255,1);
    border-radius:4px;
}
.btn_group{
  padding: 5px;
  margin-bottom: 15px;
}
@media screen and(max-width: 1366px) {
  .search_box {
     margin-left: 20px;
     margin-bottom: 5px;
    }
  .content_title{
     font-size:12px;
    
    }
    .content_text{
     font-size:12px;
   }
   .input_css{
     width:180px;
    }
    .table{
      font-size: 8px;
      
    }

}
</style>


