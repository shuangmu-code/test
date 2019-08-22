<template>
    <div class="all">
      <div class="title"><span v-if="companyId == -1">新增</span><span v-else>编辑</span>企业</div>
      <div class="content">
        <div class="step">
	        <el-steps align-center :active="active" finish-status="success" process-status="finish">
		        <el-step title="企业信息"></el-step>
		        <el-step title="负责人信息"></el-step>
		        <el-step title="签章发送配置"></el-step>
	        </el-steps>
        </div>
	    <div class="company-dome">
            <div class="input">
		        <div v-if="active==0" >
                    <el-form ref="form" :model="form" label-width="100px" >
                        <el-form-item class="nc_item" label="企业名称：">
                            <el-input size="mini" v-model="form.name" placeholder="请输入"></el-input>
                        </el-form-item>
                        <el-form-item class="nc_item" label="企业税号：">
                            <el-input size="mini" v-model="form.taxNumber" placeholder="请输入"></el-input>
                        </el-form-item>
                        <el-form-item class="nc_item" label="企业地址：">
                            <el-input size="mini" v-model="form.address" placeholder="请输入"></el-input>
                        </el-form-item>
                        <el-form-item class="nc_item" label="企业电话：">
                            <el-input size="mini" v-model="form.phone" placeholder="请输入"></el-input>
                        </el-form-item>
                        <el-form-item class="nc_item" label="开户银行：">
                            <el-input size="mini" v-model="form.bank" placeholder="请输入"></el-input>
                        </el-form-item>
                        <el-form-item class="nc_item" label="银行账号：">
                            <el-input size="mini" v-model="form.account" placeholder="请输入"></el-input>
                        </el-form-item>
                        <el-form-item class="nc_item" label="企业简拼：">
                            <el-input size="mini" v-model="form.simpleName" placeholder="请输入"></el-input>
                        </el-form-item>
                    </el-form>
		         </div>
		        <div v-if="active==1">
			         <el-form ref="form" :model="form" label-width="80px" >
                        <el-form-item class="nc_item" label="开票人：">
                            <el-input size="mini" v-model="form.drawer" placeholder="请输入"></el-input>
                        </el-form-item>
                        <el-form-item class="nc_item" label="复核人：">
                            <el-input size="mini" v-model="form.reviewer" placeholder="请输入"></el-input>
                        </el-form-item>
                        <el-form-item class="nc_item" label="收款人：">
                            <el-input size="mini" v-model="form.payee" placeholder="请输入"></el-input>
                        </el-form-item>
                    </el-form>
		        </div>
		        <div class="switch" v-if="active==2">
                   <!-- <el-tooltip :content="'Switch value: ' + value1" placement="top"> 测试switch打开时的值 -->
                    <div style="margin-bottom:5%">
                       签章发送短信：<el-switch v-model="form.noteFlag" active-color="#1890FF"  active-value="1" inactive-value="0"></el-switch>
                       
                    </div>
                    <!-- </el-tooltip> -->
                    <!-- <el-tooltip :content="'Switch value: ' + value2" placement="top">  -->
                    <div>
                       签章发送邮件：<el-switch v-model="form.emailFlag" active-color="#1890FF" active-value="1" inactive-value="0"></el-switch>
                       
                    </div>
                    <!-- </el-tooltip> -->
                </div>
                <div class="switch" v-if="active==3">
                    操作完成！
                </div>
             </div>
		    <div class="btn">
			    <el-button v-if="active<2" @click="next" size="mini" type="primary">下一步</el-button>
			    <el-button v-if="active==2" @click="submit" size="mini" type="primary">提交</el-button>
			    <el-button v-if="active==0" @click="goBack" size="mini" >取消</el-button>
			    <el-button v-if="active!=0&&active<3" @click="back" size="mini">上一步</el-button>
                <el-button v-if="active==3" @click="goBack" size="mini" type="primary">返回</el-button>
		     </div>
		 </div>
      </div>
    
     </div>
</template>

<script>
export default {
    data() {
        return{
            companyId:this.$route.params.companyId,
            active: 0,
            ok:true,
		    form:{
			   name:this.$route.params.name,
			   taxNumber:this.$route.params.taxNumber,
			   address:this.$route.params.address,
			   phone:this.$route.params.phone,
			   bank:this.$route.params.bank,
               account:this.$route.params.account,
               simpleName:this.$route.params.simpleName,
               drawer:this.$route.params.drawer,
               reviewer:this.$route.params.reviewer,
               payee:this.$route.params.payee,
               noteFlag:this.$route.params.noteFlag,
               emailFlag:this.$route.params.emailFlag
			}
        }
    },
    methods:{
        next(){
            if (this.active++ >2) this.active = 0;
        },
        submit(){
          //this.$refs['form'].validate((valid) => {
            //if (valid) {
              this.$http({
                url: this.$http.adornUrl(`/manage/company/${this.companyId == -1 ? 'save' : 'update'}`),
                method: 'post',
                data: this.$http.adornData({
                  'companyId':this.companyId == -1 ? undefined:this.companyId,
                  'name':this.form.name,
			      'taxNumber':this.form.taxNumber,
			      'address':this.form.address,
			      'phone':this.form.phone,
			      'bank':this.form.bank,
                  'account':this.form.account,
                  'simpleName':this.form.simpleName,
                  'drawer':this.form.drawer,
                  'reviewer':this.form.reviewer,
                  'payee':this.form.payee,
                  'noteFlag':this.form.noteFlag,
                  'emailFlag':this.form.emailFlag,

                })
              }).then(({data}) => {
                if (data && data.code === 0) {
                    this.$message({
                        type:'success',
                        message:'处理成功'
                    })
                } else {
                  this.$message.error(data.msg)
              }
            })
         // }
        //})
      },
	back(){
			if (this.active-- <0) this.active = 0;
        },
    goBack(){
            this.$router.replace("zzj-company_management")
        }
    }
}
</script>

<style lang="scss">
.all{
    float: left;
    width:100%;
    height:100%;
}
.title{
    height:8%;
    font-size:14px;
    font-family:PingFangSC-Regular;
    font-weight:400;
    color:rgba(39,39,39,1);
    line-height:20px;
}
.content{
    float: left;
    padding-left:4%;
    width:100%;
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
/* .step{
   	width:100%;
    display: flex;
    flex-wrap: wrap;
    justify-content: space-between;
} */
.company-dome{
    width:40%;
    margin-left:30%;
    margin-top: 16px;
}
.switch{
    margin-bottom: 10%;
}
.btn{
    float: left;
    margin-left:20%; 
}
.el-step__head.is-success{
    color: #1890FF;
    border-color: #1890FF;
    .el-step__line{
        background-color: #1890FF;
    }
}
.el-step__title.is-success {
    color: rgb(5, 5, 5);
}
.el-step__title.is-finish {
    color: rgb(5, 5, 5);
}
.el-step__head.is-finish{
    color: #1890FF;
    border-color: #1890FF;
    .el-step__icon{
        background: #1890FF;
        .el-step__icon-inner {
            z-index: 4;
            color:rgb(255, 255, 255);
        }
    }
}
    .nc_item{
        margin-bottom: 16px;
    }

</style>
