<template>
  <div class="all">
    
    <div style="position:absolute;right:80px;z-index: 999;padding:5px;border-radius:4px;background-color:rgb(255,255,255)" v-bind:class="{'dome1':use}">
      <span @click="show" class="word"><el-button type="text">放下</el-button></span>
      <span @click="show" v-html="message1"></span>
    </div>
    
    <el-card v-bind:class="{'dome1':active}" >
      <div class="mod-invoice-title">查询条件</div>
      <div class="mod-invoice-form">
        <el-form :inline="true"  size="mini" :model="dataForm" @keyup.enter.native="getDataList()">
          <el-row>
            <el-col :span="6">
              <el-form-item label="发票号码：">
                <el-input class="query-input" v-model="dataForm.invoiceNumber" placeholder="请输入"></el-input>
              </el-form-item> 
            </el-col>
            <el-col :span="6">
              <el-form-item label="交易流水号：">
                <el-input class="query-input" v-model="dataForm.businessNumber" placeholder="请输入"></el-input>
              </el-form-item >
            </el-col>
            <el-col :span="6">
              <el-form-item label="发票类型：">
                <el-radio-group class="query-input" v-model="dataForm.invoiceType" >
                  <el-radio-button label="" >全部</el-radio-button>
                  <el-radio-button label="0" >蓝票</el-radio-button>
                  <el-radio-button label="1" >红票</el-radio-button>
                </el-radio-group>  
              </el-form-item>
            </el-col>
            <el-col :span="6">
              <el-form-item label="购买方：">
                <el-radio-group class="query-input" v-model="dataForm.buyer" >
                  <el-radio-button label="" >全部</el-radio-button>
                  <el-radio-button label="企业" >企业</el-radio-button>
                  <el-radio-button label="个人" >个人</el-radio-button>
                </el-radio-group>  
              </el-form-item>
            </el-col>
          </el-row>
          <el-row>
            <el-col :span="6">
              <el-form-item label="业务类型：">  
                <el-select class="query-input" v-model="dataForm.businessType" placeholder="请选择"  >
                  <!-- 此处未绑定业务类型，待修改-->
                  <el-option label="经营租赁" value="经营租赁"></el-option>
                  <el-option label="生活服务" value="生活服务"></el-option>
                </el-select>
              </el-form-item>  
            </el-col>
            <el-col :span="6">
              <el-form-item label="开票日期：">
                <el-date-picker v-model="dataForm.date" class="query-input" type="daterange" range-separator="~" start-placeholder="开始日期" end-placeholder="结束日期"></el-date-picker>
              </el-form-item> 
            </el-col>
            <el-col :span="6"><el-form-item></el-form-item></el-col>
            <el-col :span="4">
              <el-form-item>
                <el-button type="primary" size="mini" @click="search">查询</el-button>
                <el-button type="primary" size="mini" @click="cancelInput">重置</el-button>
              </el-form-item>
            </el-col>
          <el-col :span="2">
            <span @click="showAll" class="word"><el-button type="text">收起</el-button></span>
            <span @click="showAll"  v-html="message"></span>
         </el-col>
        </el-row>
        </el-form>
      </div>
    </el-card>
    <div class="area" >
			<div  class="body">
				<div class="dome" v-for="(item,index) in datalist" :key="index" v-loading="dataListLoading">
					<span class="img2" v-if="item.gmflx=='企业'" ></span>
          <span class="img1" v-else ></span>

					<span class="text" >{{item.gfmc}}</span>
          <span class="type" style="color:rgba(62,134,247,1);">蓝票</span>
					<div class="table">
						<el-row >
							<el-col :span='8' class="column1">交易流水号</el-col><el-col :span='16' class='column2'>{{item.fplsh}}</el-col>
							<el-col :span='8' class="column1">发票代码</el-col><el-col :span='16' class='column2'>{{item.fpdm}}</el-col>
							<el-col :span='8' class="column1">发票号码</el-col><el-col :span='16' class='column2'>{{item.fphm}}</el-col>
							<el-col :span='8' class="column1">销售方</el-col><el-col :span='16' class='column2'>{{item.gsmc}}</el-col>
							<el-col :span='8' class="column1">开票日期</el-col><el-col :span='16' class='column2'>{{item.kprq | dateformat}}</el-col>
							<el-col :span='8' class="column1">业务类型</el-col><el-col :span='16' class='column2'>{{item.ywlx}}</el-col>
							<el-col :span='8' class="column1">开票金额</el-col><el-col :span='16' class='column2'>{{item.hjse}}元</el-col>
							<el-col :span='8' class="column1">发票状态</el-col><el-col :span='16' class='column2' 
              style="color:#FF9B00">未开票</el-col>
						</el-row>
				   </div>
					<el-row class="button">
						<el-button class="button1"  round  @click="toReinfo(index)">开票</el-button>
						<el-button class="button2"  round  @click="toDetails">发票详情</el-button>
					</el-row>
				</div>
      </div>
			<div class="page">
      	<el-pagination
          @current-change="currentChangeHandle"
          :current-page="currPage"
          :page-size="8"
          :total="totalPage"

      		background
      		layout="prev, pager, next"
      		>
      	</el-pagination>
      </div>		
		</div>
  </div>
</template>

<script>

  export default {
    data() {
      return {
        dataForm:{
          invoiceNumber: '',
          businessNumber: '',
          invoiceType :'',
          buyer:'',
          businessType:'',
          date:'',
          status:'',
        },
          word:'收起',
          use:true,
				  message:'<i class="el-icon-arrow-up"></i>',
          active:false,
          message1:'<i class="el-icon-arrow-down"></i>',
				  datalist:[],

          dataListLoading: false,
          currPage:1,
          totalPage:0,
			}
    },
    filters:{
      dateformat: function (time){
        let date = new Date(time)
        return date.getFullYear() + "-" + (date.getMonth() + 1) + "-" + date.getDate()
      }
    },
    activated () {
      this.getDataList()
    },
    methods: {
      getDataList(){
        this.dataListLoading = true

        var t_start = this.dataForm.date[0] != undefined ? this.dataForm.date[0].toLocaleDateString() : undefined
        var t_end = this.dataForm.date[1] != undefined ? this.dataForm.date[1].toLocaleDateString() : undefined
        this.$http({
            url: this.$http.adornUrl('/invoice/resign/list'),
            method: 'get',
            params: this.$http.adornParams({
                'page':this.dataForm.currPage,
                'limit': 8,
                'fphm':this.dataForm.invoiceNumber,
                'ywlx':this.dataForm.businessType,
                'fplsh':this.dataForm.businessNumber,
                'timeStart':t_start,
                'timeEnd':t_end,
                'kplx':this.dataForm.invoiceType,
                'fpzt':this.dataForm.status,
                'gmflx':this.dataForm.buyer
            })
      }).then(({data}) => {
          if (data && data.code === 0) {
            this.datalist = data.page.list
            this.totalPage = data.page.totalCount
          } 
          else {
            this.datalist = []
            this.totalPage = 0
          }
          this.dataListLoading = false
         })
      },
      search:function(event){
        this.getDataList()
      },
      cancelInput:function(event){
        this.dataForm.invoiceNumber=''
        this.dataForm.businessNumber=''
        this.dataForm.invoiceType=''
        this.dataForm.buyer=''
        this.dataForm.businessType=''
        this.dataForm.date=''
        this.dataForm.status=''
        this.getDataList()
      },
      show:function(even){
        this.use=true
        this.active=false
      },
      showAll:function(even){
        this.use=false
        this.active=true
      },
      toDetails:function(event){
        this.$router.replace({name:'invoice_details'})
      },
      toReinfo(index) {
        this.$confirm('确认要将该发票重新开票吗?', '温馨提示', {
          confirmButtonText: '确定',
          cancelButtonText: '取消',
          type: 'warning'
        }).then(() => {
          this.$http({
              url: this.$http.adornUrl(`/invoice/resign/makeout/${this.datalist[index].id}`),
              method: 'post',
              params: this.$http.adornParams()
          }).then(({data}) => {
              if (data && data.code === 0) {
                this.getDataList()
              } 
              else {
                this.$message({
                  type: 'error',
                  message: data.msg
                });
              }
            })
        }).catch(() => {
          this.$message({
            type: 'info',
            message: '已取消'
          });          
        });
      },
      // 当前页
      currentChangeHandle (val) {
        this.currPage = val
        this.getDataList()
      }
    }
  }
</script>

<style  lang="scss" scoped>
  .all{
    float:left;
    padding:20px;
    width:100%;
    height:100%;
    background:rgba(228,235,247,1);
  }
  .mod-invoice-title{
    font-size:18px;
    line-height: 25px;
  }
  .el-button--primary{
    background-color:#1890FF;
    border-color: #1890FF;
  }
  .el-button--primary:hover{
    background-color:#1890FF;
    border-color:  #1890FF;
  }
  .el-button--primary:focus{
    background-color: #1890FF;
    border-color: #1890FF;
  }
  .el-form-item{
    width:100%;
  }
  .query-input{
    width:250px;
  }
  //发票列表
  .area{
    float:left;
    width:100%;
    height:100%;
    background-color:rgba(228,235,247,1);
    margin-top: 16px;
  }
  .body{
  	width:100%;
    display: flex;
    flex-wrap: wrap;
    justify-content: center;
  }
  .page{
  	float:right;
  }
  .dome{
    float: left;
    width: 350px;
    height:450px ;
    background-image: url(~@/assets/img/Rectangle.png);
    background-size:100% 100%;
    &:hover {
      transform: scale(1.05);
    }
  }
  .dome1{
    display: none;
  }
  .text{
    float: left;
    margin-top: 7%;
    margin-left: 2%;  
    height:25px;
    font-size:18px;
    font-family:PingFangSC-Regular;
    font-weight:400;
    color:rgba(39,39,39,1);
  }
  .type{
    float:right;
    margin-top: 7%;
    width:18%;
    height:4%;
    font-size:14px;
    font-family:PingFangSC-Regular;
    font-weight:400;
    color:rgba(62,134,247,1);
  }
  .img1{
    float: left;
    margin-top: 5%;
    margin-left: 8%;
    margin-bottom: 5%;
    width:10%;
    height:8%;
    background-size:100% 100%;
    background-image:url(~@/assets/img/people.png);
  }
  .img2{
    float: left;
    margin-top: 5%;
    margin-left: 8%;
    margin-bottom: 5%;
    width:10%;
    height:8%;
    background-size:100% 100%;
    background-image:url(~@/assets/img/company.png);
  }  
  .table{
    float: left;
    width:84%;
    height:62%;
    margin-top:5%;
    margin-left:8%;
    border-bottom: 1px solid #E8E8E8;
  }
  .button1{
      color:#1890FF;
      border-color:#1890FF;
    }
  .button{
    float: left;
    margin-top: 3%;
    margin-left: 42%;
  }
  .word{
    width:50px;
    height:22px;
    font-size:14px;
    font-family:PingFangSC-Regular;
    color:rgba(24,144,255,1)
  }
  .column1{
    font-size:12px;
    font-family:PingFangSC-Regular;
    color:rgba(140,140,140,1);
    height:32px;
  }
  .column2{
    height:32px;
    font-size:14px;
    font-family:PingFangSC-Regular;
    font-weight:400;
    color:rgba(39,39,39,1);
    text-align:right;
  }
  @media screen and (max-width: 1680px){
    .dome{
      width: 320px;
      height:390px ;
    }
    .query-input{
      width:200px;
    }
  }
  @media screen and (max-width: 1620px){
    .mod-invoice-title{
      font-size:15px;
      line-height: 21px;
    }
    .word{
      font-size:10px;
    }
   .dome{
      width: 300px;
      height:375px ;
    }
    .text{
      font-size: 15px;
    }
    .type{
      font-size:12px;
    }
    .img{
      width:28px;
      height:28px;
    }
    .column1{
      font-size:12px;
      font-family:PingFangSC-Regular;
      color:rgba(140,140,140,1);
      height:28px;
    }
    .column2{
      height:28px;
      font-size:12px;
      font-family:PingFangSC-Regular;
      font-weight:400;
      color:rgba(39,39,39,1);
      text-align:right;
    }
    .button1,.button2{
      width:75px;
      height:25px;
      font-size:12px;
      padding-top:6px;
      padding-left:23px;
    }
    .button2{
      padding-left:12px;
      margin-left:8px;
    }

  }
  @media screen and (max-width: 1440px){
    .dome{
      width: 270px;
      height:375px ;
    }
  }
  @media screen and (max-width: 1370px){
    .all{
      padding:14px;
    }
    .mod-invoice-title{
      font-size:13px;
      line-height: 18px;
    }
    .word{
      font-size:10px;
    }
   .dome{
      width: 250px;
      height:320px ;
    }
    .text{
      font-size: 13px;
    }
    .type{
      font-size:10px;
    }
    .img{
      width:24px;
      height:24px;
    }
    .column1{
      font-size:10px;
      font-family:PingFangSC-Regular;
      color:rgba(140,140,140,1);
      height:24px;
    }
    .column2{
      height:24px;
      font-size:10px;
      font-family:PingFangSC-Regular;
      font-weight:400;
      color:rgba(39,39,39,1);
      text-align:right;
    }
    .button1,.button2{
      width:65px;
      height:21px;
      font-size:10px;
      padding-top:4px;
      padding-left:20px;
    }
    .button2{
      padding-left:8px;
      margin-left:2px;
    }
  }
  @media screen and (max-width: 1280px){
    .mod-invoice-title{
      font-size:12px;
      line-height: 17px;
    }
    .word{
      font-size:9px;
    }
   .dome{
      width: 230px;
      height:288px ;
    }
    .text{
      font-size: 12px;
    }
    .type{
      font-size:9px;
    }
    .img{
      width:23px;
      height:23px;
    }
    .column1{
      font-size:9px;
      font-family:PingFangSC-Regular;
      color:rgba(140,140,140,1);
      height:20px;
    }
    .column2{
      height:20px;
      font-size:9px;
      font-family:PingFangSC-Regular;
      font-weight:400;
      color:rgba(39,39,39,1);
      text-align:right;
    }
    .button1,.button2{
      width:50px;
      height:20px;
      font-size:9px;
      padding-top:3px;
      padding-left:13px;
    }
    .button2{
      padding-left:5px;
      margin-left:3px;
    }
  }
  </style>