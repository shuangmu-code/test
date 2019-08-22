<template>

   <div class="content">
      <div class="content_title">新增商铺</div>
      <el-form v-model="dataForm" :label-position="left" label-width="120px" class="add_box">
             <el-form-item label="商铺名称：">
               <el-input v-model="dataForm.shop_name"></el-input>
             </el-form-item>
             <el-form-item label="商铺编号：">
               <el-input v-model="dataForm.shop_number" ></el-input>
             </el-form-item>
             <el-form-item label="销方名称：">
               <el-input v-model="dataForm.sale_name" ></el-input>
             </el-form-item>
             <el-form-item label="销方税号：">
               <el-input v-model="dataForm.sale_duty_paragraph"  ></el-input>
             </el-form-item>
             <el-form-item label="销方地址：">
               <el-input v-model="dataForm.sale_address" ></el-input>
             </el-form-item>
             <el-form-item label="销方电话：">
               <el-input v-model="dataForm.sale_phone_number" ></el-input>
             </el-form-item>
             <el-form-item label="银行卡号：">
               <el-input v-model="dataForm.sale_bank_card_number"></el-input>
             </el-form-item>
            
            <el-form-item label="">
               <el-button type="primary" class="content_button" style="background:rgba(24,144,255,1);" @click="submit">提交</el-button>
               <el-button type="primary" class="content_button" style="background:rgba(255,255,255,1);color:rgba(0,0,0,0.65);border:1px solid rgba(217,217,217,1);" @click="cancel">取消</el-button>
            </el-form-item>
            
      </el-form>
   </div>
</template>

<script>
   export default {
            data(){
             return{
             dataForm:{
                shop_name:'',
                shop_number:'',
                sale_number:'',
                sale_duty_paragraph:'',
                sale_address:'',
                sale_phone_number:'',
                sale_bank_card_number:'',
             },
            }
         },
         submit:function(event){
          alert('提交')
        },
        cancel:function(event){
          alert('取消')
      },
}
</script>

<style>
      .content_title{
         padding: 5px;
         font-size: 18px;
         font-weight: 550;
      }
      .add_box{
         width: 40%;
         margin-left: 10%;
      }
 @media screen and (max-width: 1500px){
     .content_title{
        font-size: 13px;
      }
 }
  @media screen and (max-width: 1300px){
     .content_title{
        font-size: 10px;
      }
 }
</style>