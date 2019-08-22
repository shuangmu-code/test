<template>
 <div class="page-add--category">
     <div class="padge-title"><span v-if="categoryId == -1">新增</span><span v-else>编辑</span>类目</div>

      <div class="box">
        <div class="card-title">请选择企业</div>

		<el-form label-position="right" label-width="100px" class="select_box">
             <el-form-item class="form-item" label="企业名称：">
               <el-select v-model="select_company" @change="companyChange" value-key="companyId" filterable placeholder="请选择" size="mini">
                <el-option v-for="item in company_source" :key="item.companyId" :value="item" :label="item.name">
                </el-option>
              </el-select>
             </el-form-item>
             <el-form-item class="form-item" label="企业税号：">
               {{data_select.taxNumber}}
             </el-form-item>
             <el-form-item class="form-item" label="企业地址：">
               {{data_select.address}}
             </el-form-item>
			 <el-form-item class="form-item" label="企业电话：">
               {{data_select.phone}}
             </el-form-item>
			 <el-form-item class="form-item" label="开户银行：">
               {{data_select.bank}}
             </el-form-item>
			 <el-form-item class="form-item" label="银行账号：">
               {{data_select.account}}
             </el-form-item>
		</el-form>
     </div>
	
	<div class="box">
        <div class="card-title">请填写类目信息</div>

		<el-form v-model="data_form2" label-position="right" label-width="120px" class="add_box">
             <el-form-item class="form-item" label="经营类目：">
               <el-input v-model="data_form2.categoryName" placeholder="请输入" size="mini"></el-input>
			   <div class="hint">格式如：<span style="color:red;">*服务费*印刷费；</span>前后用*+商品简码连接</div>
             </el-form-item>
             <el-form-item class="form-item" label="类目编号：">
               <el-input v-model="data_form2.categoryId" placeholder="请输入" :disabled="true" size="mini"></el-input>
             </el-form-item>
             <el-form-item class="form-item" label="税收分类编码：">
				<el-select v-model="data_form2.taxCode" filterable remote :remote-method="searchTaxCode" placeholder="请选择" size="mini">
					<el-option 
						v-for="item in taxCode_source"
						:key="item.taxCode"
						:label="item.taxCode"
						:value="item.taxCode">
					</el-option>
				</el-select>
             </el-form-item>
			 <el-form-item class="form-item" label="默认税率：">
                <el-input-number v-model="data_form2.taxRate"  :precision="2" :step="0.1" :max="10" :min="0" size="mini"></el-input-number>
                <label class="inpute_number_lable">%</label><!--- 此处数据未除以100，在后续运算时再处理-->
             </el-form-item>
			 <el-form-item class="form-item" label="经营地址：">
               <el-input v-model="data_form2.address" placeholder="请输入" size="mini"></el-input>
             </el-form-item>
			 <el-form-item class="form-item" label="经营电话：">
               <el-input v-model="data_form2.phone" placeholder="请输入" size="mini"></el-input>
             </el-form-item>
			<el-form-item class="form-item">
               <el-button type="primary"  @click="dataFormSubmit()" size="mini">提交</el-button>
        		<el-button size="mini">取消</el-button>
            </el-form-item>
			 
		</el-form>
     </div>

 </div>
</template>
<script>
export default {
    data(){
      return{
		categoryId:this.$route.params.id,
		company_source:[],
		select_company:'',
        data_select:{},
        taxCode_source:[],
        data_form2:{
          categoryName:'',
          categoryId:'',
          taxCode:'',
          taxRate:'',
          address:'',
		  phone:'',
		  companyId:0
        }
      }
	},
	mounted: function() {
		this.init()
		this.searchTaxCode()
    },
    methods:{
		init(){
			if (this.categoryId != -1) {
					this.$http({
					url: this.$http.adornUrl(`/manage/category/info/${this.categoryId}`),
					method: 'get',
					params: this.$http.adornParams()
					}).then(({data}) => {
						if (data && data.code === 0) {
							this.data_form2.categoryName = data.item.name
							this.data_form2.categoryId = data.item.categoryId
							this.data_form2.taxCode = data.item.taxCode
							this.data_form2.taxRate = data.item.taxRate
							this.data_form2.address = data.item.address
							this.data_form2.phone = data.item.phone
							this.data_form2.companyId = data.item.companyId

							//todo 选择企业
							this.$http({
								url: this.$http.adornUrl('/manage/company/select'),
								method: 'get',
								params: this.$http.adornParams()
							}).then(({data}) => {
								this.company_source = data && data.code === 0 ? data.list : []

								this.company_source.forEach(Element =>{
									if(Element.companyId == this.data_form2.companyId){
										this.data_select=Element
										this.select_company=Element.name
									}
								})
							})

						}
					})
			}else{
				this.$http({
					url: this.$http.adornUrl('/manage/company/select'),
					method: 'get',
					params: this.$http.adornParams()
				}).then(({data}) => {
					this.company_source = data && data.code === 0 ? data.list : []
				})
			}

		},
		searchTaxCode(query){
			this.$http({
				url: this.$http.adornUrl('/manage/commodity/select'),
				method: 'get',
				params: this.$http.adornParams({
					'tax_code': query
				})
			}).then(({data}) => {
				this.taxCode_source = data && data.code === 0 ? data.data : []
			})
		},
		dataFormSubmit(){
			this.$confirm(`确认保存类目信息吗?`, '保存信息', {
				confirmButtonText: '确定',
				cancelButtonText: '取消',
				type: 'info'
			}).then(() => {
				this.$http({
					url: this.$http.adornUrl(`/manage/category/${!this.data_form2.categoryId ? 'save' : 'update'}`),
					method: 'post',
					data: this.$http.adornData({
						'categoryId': this.data_form2.categoryId || undefined,
						'name':  this.data_form2.categoryName,
						'taxCode':  this.data_form2.taxCode,
						'taxRate':  this.data_form2.taxRate,
						'classify':  '',
						'address':  this.data_form2.address,
						'phone':  this.data_form2.phone,
						'companyId':  this.data_select.companyId,
					})
				}).then(({data}) => {
					if (data && data.code === 0) {
						this.$message({
							message: '操作成功',
							type: 'success',
							duration: 1500,
						})
					}
					else {
						this.$message.error(data.msg)
					}
				})
			})
		},
		companyChange(item){
			this.data_select=item
			this.data_form2.companyId=item.companyId
		}
    }
}
</script>
<style lang="scss" scoped>
	.padge-title{
		font-size: 18px;
		padding: 5px 10px;
		margin-bottom: 20px;
	}
	.box{
		width:40%;
		float: left;
		margin-left: 10px;
		margin-right: 20px;
		border: 1px solid #D9D9D9;
		border-radius: 10px;
		min-height: 420px;
  	}
	.card-title{
		height:45px;
		line-height: 45px;
		padding-left: 20px;
		font-size: 16px;
		color: #000;
		background-color: #FBFCFF;
		border-radius: 10px 10px 0 0 ;
		border-bottom: 1px solid rgba(217,217,217,1);
    margin-bottom: 20px;
	}
	.add_box{
		width: 100%;
		padding: 5px 10px;
	}
	.form-item{
		margin-bottom: 8px;
	}
	.hint{
		font-size: 12px;
	}
	
</style>
