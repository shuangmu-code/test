<template>
    <div class="bg_content">
        <el-card>
            <div class="title">查询条件</div>
            <el-form v-model="dataform" inline class="form">
                <el-form-item label="企业名称">
                    <el-input v-model="dataform.company_name" placeholder="请输入"></el-input>
                </el-form-item>
                <el-form-item label="企业税号" >
                    <el-input v-model="dataform.company_rate_number" placeholder="请输入"></el-input>
                </el-form-item>
                <el-button type="primary" @click="getdataList()" >查询</el-button>
                <el-button @click="reset">重置</el-button>
            </el-form>
        </el-card>
        <el-button type="primary" class="button" @click="add_company('-1')" icon="el-icon-plus">新增企业</el-button>
        <div class="area" v-loading="dataListLoading">
            <el-card v-if="company_information.length<1" style="text-align:center" >
                <div class="nodata"></div> 
                <div class="nd_title">暂时没有企业</div>
                <div class="nd_sub_title">还没有企业，点击下方按钮增加企业</div>
               <el-button type="primary" class="button" @click="add_company('-1')" icon="el-icon-plus">新增企业</el-button>
            </el-card>
            <el-card v-else class="company-card" v-for="(item,index) in company_information" :key="index">

                <div slot="header" class="clearfix card-header">
                    <div class="img"></div>
                    <div class="lable_title"><span>{{item.name}}</span></div>

                    <div class="header-btn">
                        <el-button  @click="delete_company(index)" size="mini" round>删除</el-button>
                        <el-button type="primary" @click="add_company(index)" size="mini" round>编辑</el-button>
                    </div>
                </div>

                <el-row>
                    <el-col :span="12">
                         <div class="col-name">企业信息</div>
                         <ul>
                             <li><span class="col-item">企业税号</span><span class="col-value">{{item.taxNumber}}</span></li>
                             <li><span class="col-item">企业地址</span><span class="col-value">{{item.address}}</span></li>
                             <li><span class="col-item">企业电话</span><span class="col-value">{{item.phone}}</span></li>
                             <li><span class="col-item">开户银行</span><span class="col-value">{{item.bank}}</span></li>
                             <li><span class="col-item">银行账号</span><span class="col-value">{{item.account}}</span></li>
                             <li><span class="col-item">企业简拼</span><span class="col-value">{{item.simpleName}}</span></li>
                         </ul>
                    </el-col>
                    <el-col :span="5">
                         <div class="col-name">负责人信息</div>
                         <ul>
                            <li><span class="col-item">开票人</span><span class="col-value">{{item.drawer}}</span></li>
                            <li><span class="col-item">复核人</span><span class="col-value">{{item.reviewer}}</span></li>
                            <li><span class="col-item">收款人</span><span class="col-value">{{item.payee}}</span></li>
                         </ul>
                    </el-col>
                   
                   <el-col :span="7">
                         <div class="col-name">签章发送配置</div>
                         <ul>
                            <li><span class="col-item">签章发送信息</span><span class="col-value" v-if="item.noteFlag==1">启用</span><span class="col-value" v-else>禁用</span></li>
                            <li><span class="col-item">签章发送邮件</span><span class="col-value" v-if="item.emailFlag==1">启用</span><span class="col-value" v-else>禁用</span></li>
                         </ul>
                    </el-col>
                </el-row>

            </el-card>
        </div>
    </div>
</template>
<script>
export default {
    data(){
        return{
            dataform:{
                company_name:'',
                company_rate_number:'',
            },
            company_information:[],
            dataListLoading: false,
            companyId:[],
        }
    },
    activated () {
      this.getdataList()
    },
    methods:{
       getdataList(){
           this.dataListLoading = true
           this.$http({
               url: this.$http.adornUrl('/manage/company/list'),
               method: 'get',
               params: this.$http.adornParams({
                   'page': '1',
                   'limit': '10',
                   'name':this.dataform.company_name,
                   'taxNumber':this.dataform.company_rate_number,
               })
           }).then(({data}) =>{
                 if (data && data.code === 0) {
                     this.company_information = data.page.list
                    
                    } 
                 else {
                       this.company_information = []
                    }
          this.dataListLoading = false
         })
         },

       reset:function(event){
           this.dataform.company_name='';
           this.dataform.company_rate_number='';
         },

       add_company(index){
           if(index==='-1'){
                this.$router.replace({
                    name: 'company_add',
                    params:{
                        companyId:-1,
                    }
                })
           }
           else {
                this.$router.replace({
                    name: 'company_add',
                    params:{
                    companyId:this.company_information[index].companyId,
                    name:this.company_information[index].name,
			        taxNumber:this.company_information[index].taxNumber,
			        address:this.company_information[index].address,
			        phone:this.company_information[index].phone,
			        bank:this.company_information[index].bank,
                    account:this.company_information[index].account,
                    simpleName:this.company_information[index].simpleName,
                    drawer:this.company_information[index].drawer,
                    reviewer:this.company_information[index].reviewer,
                    payee:this.company_information[index].payee,
                    noteFlag:this.company_information[index].noteFlag,
                    emailFlag:this.company_information[index].emailFlag
                }
                })
           }

         },
       delete_company(index){
           this.companyId=[this.company_information[index].companyId],
           this.$confirm('确认要删除当前企业信息吗？', '温馨提示', {
           confirmButtonText: '确定',
           cancelButtonText: '取消',
           type: 'warning',
        }).then(() => {
             this.$http({
                  
                  url: this.$http.adornUrl('/manage/company/delete'),
                  method: 'post',
                  data: this.$http.adornData(this.companyId,false)
             }).then(({data})=>{
                  if (data && data.code === 0) {
                       this.$message({
                       type: 'success',
                       message: '已操作成功!',
                       });
                      this.getdataList();
                    }

                  else{
                      this.$message.error(data.msg)
                  }
               })

        }).catch(() => {

              this.$message({
                 type: 'info',
                 message: '已取消'
            });          
          });

       },
    }
}
</script>
<style lang="scss" scoped>

.title{
    margin-top:10px;
    margin-left:10px;
    font-size:18px;
    font-family:PingFangSC-Regular;
    font-weight:400;
    color:rgba(39,39,39,1);
    line-height:25px;
}
.form{
    margin-top:10px;
    margin-left:10px;
    width:100%;
}

.button{
    margin-top:15px;
    margin-left:20px;
}
.area{
      margin-top:20px;
      width:100%;
  }
    .company-card{
        margin-bottom: 20px;
    }
    .card-header{
        height:36px;
        line-height: 36px;
    }
    .img{
        width:34px;
        height:34px;
        float: left;
        background-size:100% 100%;
        background-image:url(~@/assets/img/company.png);
    }
    .lable_title{
        height: 36px;
        line-height: 36px;
        float: left;
        margin-left: 8px;
        font-size:18px;
        font-family:PingFangSC-Regular;
        color: #272727;
    }
    .header-btn{
        float: right;
    }

    ul {
        padding: 0;
    }
    ul li{
        list-style: none;
        width:100%;
        line-height: 28px;
    }
    .col-item{
        margin-right: 10px;
        color:#8C8C8C;
        font-family:PingFangSC-Regular;
    }
    .col-value{
        color: #272727;
        font-family:PingFangSC-Regular;
    }
    .nodata{
        width:254px;
        height: 150px;
        margin: 0 auto;
        margin-top: 170px;
        background-image:url(~@/assets/img/Group4.png);
        background-size:100% 100%;
    }
    .nd_title{
        font-size: 20px;
        font-family:PingFangSC-Regular;
        font-weight:400;
        color:rgba(39,39,39,1);
        height:28px;
        line-height:28px;
    }
    .nd_sub_title{
        height:20px;
        margin-top: 10px;
        font-size:14px;
        font-family:PingFangSC-Regular;
        font-weight:400;
        color:rgba(140,140,140,1);
        line-height:20px;
    }
</style>
