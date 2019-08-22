<template>
   <div class="manual-bill">
       <div>
            <div class="manual-bill-title">手动开票</div>
            <div class="manual-btn-card">
                <el-button class="add-btn" @click="handleAdd" plain>添加明细</el-button>
                <el-button class="dele-btn" @click="handleDel" plain>删除明细</el-button>
                <el-button class="discount-btn" @click="addDiscount" plain>折扣行</el-button>
                <el-button class="reset-btn" plain @click="resetBtn">重  置</el-button>
                <el-button class="make-bill-btn" style="primary" @click="dialogVisible = true">开票签章</el-button>
            </div>
            <div class="manual-bill-body">
                <div class="manual-bill-body-left">
                    <div class="left-circle" style="margin-top:10px"></div>
                    <div class="left-circle"></div>
                    <div class="left-circle"></div>
                    <div class="left-circle"></div>
                    <div class="left-circle"></div>
                    <div class="left-circle"></div>
                    <div class="left-circle"></div>
                    <div class="left-circle"></div>
                    <div class="left-circle"></div>
                    <div class="left-circle"></div>
                    <div class="left-circle"></div>
                    <div class="left-circle"></div>
                    <div class="left-circle"></div>
                    <div class="left-circle"></div>
                    <div class="left-circle"></div>
                    <div class="left-circle"></div>
                </div>
                <div class="manual-bill-body-center">
                    <div class="manual-bill-body-title"></div>
                    <div class="manual-bill-body-tables">
                        <el-row style="heigh:100%;width:100%">
                            <el-col :span="10">
                                <div class="top-label-style" style="text-align:right">开票日期：</div>
                            </el-col>
                            <el-col :span="14">
                                <el-date-picker class="makeout-date-piker" v-model="formMessage.dateSelect" type="date"  placeholder="选择日期"></el-date-picker>
                            </el-col>
                        </el-row>
                    </div>
                    <table class="manual-bill-table-top">
                        <tr class="manual-bill-table-top-tr">
                            <td class="text-purchaser">购买方</td>
                            <td class="basic-information-top">
                                <el-form :model="formMessage">
                                    <el-row class="basic-information-top-row">
                                        <el-col :span="6">
                                            <div class="top-label-style">名&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;称：</div>
                                        </el-col>
                                        <el-col :span="18" class="last-col"> 
                                            <input v-model="formMessage.applicantName" class="top-input-style"  placeholder="  请输入"/>
                                        </el-col>
                                    </el-row>
                                    <el-row class="basic-information-top-row">
                                        <el-col :span="6">
                                            <div class="top-label-style">纳税人识别号：</div>
                                        </el-col>
                                        <el-col :span="18" class="last-col">
                                            <input v-model="formMessage.applicantIdentifier" class="top-input-style"  placeholder="  请输入"/>
                                        </el-col>
                                    </el-row>
                                    <el-row class="basic-information-top-row">
                                        <el-col :span="6">
                                            <div class="top-label-style">地 址、&nbsp;&nbsp;电 话：</div>
                                        </el-col>
                                        <el-col :span="11">
                                            <input v-model="formMessage.applicantAddrress" class="top-input-style" placeholder="  请输入地址"/>
                                        </el-col>
                                        <el-col :span="6" class="last-col">
                                            <input v-model="formMessage.applicantCompanyTel" class="top-input-style" style="margin-left:20px" placeholder="  请输入电话"/>
                                        </el-col>
                                    </el-row>
                                    <el-row class="basic-information-top-row">
                                        <el-col :span="6">
                                            <div class="top-label-style">开户行及账号：</div>
                                        </el-col>
                                        <el-col :span="11">
                                            <input v-model="formMessage.applicantAccountBank" class="top-input-style" placeholder="  请输入开户行"/>
                                        </el-col>
                                        <el-col :span="6" class="last-col">
                                            <input v-model="formMessage.applicantAccountNum" class="top-input-style" style="margin-left:20px" placeholder="  请输入账号"/>
                                        </el-col>
                                    </el-row>
                                </el-form>
                            </td>
                            <td class="text-purchaser">联系方式</td>
                            <td class="basic-information-top"> 
                                <input v-model="formMessage.applicantTelphone" class="basic-information-top-input-style" placeholder="  请输入购方手机" />
                            </td>
                        </tr>
                    </table>   
                    <div class="manual-bill-table-detail-layout" >
                        <table class="manual-bill-table-detail" id="table-detail">
                            <tr class="table-detail-style-tr1">
                                <td  class="detail-label-style-td3">
                                    <div class="detail-label-style-td1"></div>
                                    <div class="detail-label-style-td2">货物或应税劳务、服务名称</div>
                                </td>
                                <td class="detail-label-style">规&nbsp;格&nbsp;型&nbsp;号</td>
                                <td class="detail-label-style">单&nbsp;&nbsp;位</td>
                                <td class="detail-label-style">数&nbsp;&nbsp;量</td>
                                <td class="detail-label-style">单&nbsp;价(含税)</td>
                                <td class="detail-label-style">金&nbsp;额(含税)</td>
                                <td class="detail-label-style">税&nbsp;&nbsp;率</td>
                                <td class="detail-label-style">税&nbsp;&nbsp;额</td>
                            </tr>
                            <tr class="table-detail-style-tr2" v-for="(item,index) in total_details" :key="index">
                                <td class="detail-label-style-td3" style="border:1px solid #8C8C8C;" v-if="!item.discount">
                                    <input class="detail-label-style-td1" type="checkbox" v-model="item.check">
                                    <div class="detail-label-style-td2" @click ="selectCommodityHandle(index)">
                                        <input class="select-input" 
                                               v-model="item.Name" 
                                               placeholder="请选择"/>
                                        <i class="el-icon-caret-bottom"></i>
                                    </div>
                                </td>
                                <td v-else style="border:1px solid #8C8C8C;" >
                                    <input class="detail-label-style-td1" type="checkbox" v-model="item.check">
                                    <span style="width:80%;border:0;background-color:transparent;font-size:14px;">折扣行</span>
                                </td>
                                <td class="detail-label-style" style="border:1px solid #8C8C8C;">
                                    <input v-model="item.Type" class="detail-input-style" placeholder="  输入"/>
                                </td>
                                <td class="detail-label-style" style="border:1px solid #8C8C8C;">
                                    <input v-model="item.Unit" class="detail-input-style" placeholder="  输入"/>
                                </td>
                                <td class="detail-label-style" style="border:1px solid #8C8C8C;">
                                    <input v-model="item.number" @input="calcPrice(index)" class="detail-input-style" placeholder="  输入"/>
                                </td>
                                <td class="detail-label-style" style="border:1px solid #8C8C8C;">
                                    <input v-model="item.Unitprice" @input="calcPrice(index)" class="detail-input-style" placeholder="  输入"/>
                                </td>
                                <td class="detail-label-style" style="border:1px solid #8C8C8C;">
                                    <input v-model="item.Sumprice" class="detail-input-style" placeholder="  请输入"/>
                                </td>
                                <td class="detail-label-style" style="border:1px solid #8C8C8C;"  >
                                    <!--此处有点小问题：若修改税率，税率框只剩下%时，该行税额以及合计行出现 NaN-->
                                    <input class="detail-input-style" style="text-align:center" v-model="item.Taxrate" @input="calcPrice(index)"/>
                                </td>
                                <td class="detail-label-style" style="border:1px solid #8C8C8C;">
                                    <input v-model="item.Tax" class="detail-input-style" placeholder="  请输入"/>    
                                </td>
                            </tr>
                        </table>
                    </div> 
                    <table class="manual-bill-table-bottom">
                        <tr class="table-bottom-tr1">
                            <td class="table-bottom-total">合&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;计</td>
                            <td>
                                <el-row class="table-bottom-totalnumber">
                                    <el-col :span="12" class="table-bottom-text">¥ {{totalSumPrice}}</el-col>
                                    <el-col :span="12" class="table-bottom-text">¥ {{totalTax}}</el-col>
                                </el-row>
                            </td>
                        </tr>
                        <tr class="table-bottom-tr2">
                            <td class="table-bottom-total">价税合计(大写)</td>
                            <td>
                                <el-row class="table-bottom-totalnumber">
                                    <el-col :span="12" class="table-bottom-text" style="text-align:left;font-family: PingFangSC-Regular, sans-serif;">{{totalTaxPriceBig }}</el-col>
                                    <el-col :span="4" class="table-bottom-text" style="color:#B1743A">(小写)</el-col>
                                    <el-col :span="8" class="table-bottom-text" style="text-align:left">&nbsp;&nbsp;¥{{totalTaxPriceSmall}}</el-col>
                                </el-row>
                            </td>
                        </tr>
                    </table>
                    <table class="manual-bill-table-seller">
                         <tr class="manual-bill-table-top-tr">
                            <td class="text-purchaser">销售方</td>
                            <td class="basic-information-top">
                                <el-form :model="formMessage">
                                    <el-row class="basic-information-top-row">
                                        <el-col :span="6">
                                            <div class="top-label-style">名&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;称：</div>
                                        </el-col>
                                        <el-col :span="18" class="last-col"> 
                                            <input v-model="formMessage.sellerName" class="bottom-input-style" disabled/>
                                        </el-col>
                                    </el-row>
                                    <el-row class="basic-information-top-row">
                                        <el-col :span="6">
                                            <div class="top-label-style">纳税人识别号：</div>
                                        </el-col>
                                        <el-col :span="18" class="last-col">
                                            <input v-model="formMessage.sellerIdentifier" class="bottom-input-style" disabled/>
                                        </el-col>
                                    </el-row>
                                    <el-row class="basic-information-top-row">
                                        <el-col :span="6">
                                            <div class="top-label-style">地 址、&nbsp;&nbsp;电 话：</div>
                                        </el-col>
                                        <el-col :span="11">
                                            <input v-model="formMessage.sellerAddrress" class="bottom-input-style" disabled/>
                                        </el-col>
                                        <el-col :span="6" class="last-col">
                                            <input v-model="formMessage.sellerTelphone" class="bottom-input-style" style="margin-left:20px" disabled/>
                                        </el-col>
                                    </el-row>
                                    <el-row class="basic-information-top-row">
                                        <el-col :span="6">
                                            <div class="top-label-style">开户行及账号：</div>
                                        </el-col>
                                        <el-col :span="11">
                                            <input v-model="formMessage.sellerAccountBank" class="bottom-input-style" disabled/>
                                        </el-col>
                                        <el-col :span="6" class="last-col">
                                            <input v-model="formMessage.sellerAccountNum" class="bottom-input-style" disabled style="margin-left:20px" />
                                        </el-col>
                                    </el-row>
                                </el-form>
                            </td>
                            <td class="text-purchaser">备注</td>
                            <td class="basic-information-top" style="padding:10px"> 
                                <!--<el-input type="textarea" :rows="6" v-model="remarkTextarea" class="information_note"></el-input>-->
                                <textarea v-model="remarkTextarea" wrap="virtual" class="information_note" placeholder="备注"></textarea>
                            </td>
                        </tr>
                    </table>    
                    <div class="manual-bill-table-bottom-label">
                        <el-row>
                            <el-col :span="2">
                                <div class="top-label-style">收款人：</div>
                            </el-col>
                            <el-col :span="4">
                                <input v-model="formMessage.chamberlain" class="top-input-style" placeholder="  请输入"/>
                            </el-col>
                            <el-col :span="2">
                                <div class="top-label-style">复核：</div>
                            </el-col>
                            <el-col :span="4">
                                <input v-model="formMessage.review" class="top-input-style" placeholder="  请输入"/>
                            </el-col>
                            <el-col :span="2">
                                <div class="top-label-style">开票人：</div>
                            </el-col>
                            <el-col :span="4">
                                <input v-model="formMessage.issuer" class="top-input-style" placeholder="  请输入"/>
                            </el-col>
                            <el-col :span="3">
                                <div class="top-label-style">销售方：(章)</div>
                            </el-col>
                            <el-col :span="3">
                                <input class="top-input-style" style="border-bottom:0" disabled/>
                            </el-col>
                        </el-row>
                    </div>
                </div>   
                <div class="manual-bill-body-left">
                    <div class="right-circle" style="margin-top:10px"></div>
                    <div class="right-circle"></div>
                    <div class="right-circle"></div>
                    <div class="right-circle"></div>
                    <div class="right-circle"></div>
                    <div class="right-circle"></div>
                    <div class="right-circle"></div>
                    <div class="right-circle"></div>
                    <div class="right-circle"></div>
                    <div class="right-circle"></div>
                    <div class="right-circle"></div>
                    <div class="right-circle"></div>
                    <div class="right-circle"></div>
                    <div class="right-circle"></div>
                    <div class="right-circle"></div>
                    <div class="right-circle"></div>
                </div> 
            </div>
        </div>

        <el-dialog :visible.sync="dialogVisible" width="30%" top="36vh" class="makeout--dialog">
            <span slot="title"><i class="el-notification__icon el-icon-warning" style="color:#FF9B00"></i>温馨提示</span>
            <span style="padding-left:30px">确认开票吗？</span>
            <span slot="footer" class="dialog-footer">
                <el-button @click="dialogVisible = false">取 消</el-button>
                <el-button type="primary" @click="handleMakeOut">
                    <span v-if="!mekeOutLoading">确 定</span>
                    <span v-else>执行中…</span>
                </el-button>
            </span>
        </el-dialog>

        <el-dialog :visible.sync="dialogSuccessVisible" width="30%" top="36vh" class="makeout-ok--dialog" center>
            <i class="el-notification__icon el-icon-success dialog-icon"></i>
            <div class="dialog-info">开票成功!</div>
            
            <span slot="footer" class="dialog-footer">
                <el-button type="primary" @click="dialogSuccessVisible = false; viewInvoice()" style="width:50%">查看发票</el-button>
            </span>
        </el-dialog>
        <!--监听子组件的自定义事件change，并添加一个响应该事件的方法getSelectedData-->
        <select-commodity v-if="selectCommodityVisible" @change="getSelectedData" ref="selectCommodity" ></select-commodity>

    </div>
</template>

<script>
import { METHODS } from 'http';
import { strictEqual } from 'assert';
import selectCommodity from './selectCommodity'
export default {
    provide(){
        return{
            refresh:this.refresh
        }
    },
    inject:['refresh'],
    components: {
      selectCommodity
    },
    data(){
        return{
            dialogVisible:false,
            dialogSuccessVisible:false,
            mekeOutLoading:false,
            selectCommodityVisible:false,

            currentIndex:0,

            formMessage:{
                dateSelect:'',  //开票日期
                applicantName:'',    // 购买方名称    
                applicantIdentifier:'',   //购买方纳税人识别号
                applicantAddrress:'',  //购买方地址
                applicantCompanyTel:'',  //购买方公司电话
                applicantTelphone:'',    //购买方申请人电话
                applicantAccountBank:'',  //购买方开户行
                applicantAccountNum:'',   //购买方开户行账号
                sellerName:'', //销售方名称
                sellerIdentifier:'', //销售方纳税人识别号
                sellerAddrress:'',  //销售方地址
                sellerTelphone:'',  //销售方电话
                sellerAccountBank:'', //销售方开户行
                sellerAccountNum:'',  //销售方开户行账号
                chamberlain:'',    //收款人
                review:'',   //复核
                issuer:''  //销售方（章）
            },
            total_details:[
                {
                    check:false,  
                    Name:'',  //商品名称
                    Type:'',  //规格型号
                    Unit:'',  //单位
                    number:'',  //数量
                    Unitprice:'',  //单价（含税）
                    Sumprice:'',  //金额（含税）
                    Taxrate:'0%',  //税率
                    Tax:'',    ///税额
                    discount:false,
                }
            ],
            totalSumPrice:'0.00',
            totalTax:'0.00',
            totalTaxPriceBig:'零圆整',
            totalTaxPriceSmall:'0.00',
            remarkTextarea:'',   
        }
    },
    // filters: {
    //     TaxFormat: function (value) {
    //         if (!value) return '0%';
    //         if (value=='') return '0%';

    //         value=value.toString()

    //         return value
    //     }
    // },
    mounted: function() {
        this.$http({
            url: this.$http.adornUrl('/manage/company/select'),
            method: 'get',
            params: this.$http.adornParams()
        }).then(({data}) => {
            if (data && data.code === 0) {
                this.formMessage.sellerName=data.list[0].name
                this.formMessage.sellerIdentifier=data.list[0].taxNumber
                this.formMessage.sellerAddrress=data.list[0].address
                this.formMessage.sellerTelphone=data.list[0].phone
                this.formMessage.sellerAccountBank=data.list[0].bank
                this.formMessage.sellerAccountNum=data.list[0].account
            }
        })
    },
    methods:{
        calcPrice(index){
            let number=this.total_details[index].number
            let uPrice=this.total_details[index].Unitprice
            let rate=this.total_details[index].Taxrate == '' ? 0 : this.total_details[index].Taxrate
            let ifdiscount=this.total_details[index].discount

            if(number != '' && uPrice != ''){
                let sum= parseInt(number) * parseFloat(uPrice)
                rate=parseInt(rate) * 0.01
                let tax=sum * rate/(rate + 1)
                tax = tax.toFixed(2) //保留两位小数
                
                this.total_details[index].Sumprice=sum
                this.total_details[index].Tax=tax
            }else{
                this.total_details[index].Sumprice=''
                this.total_details[index].Tax=''
            } 
            this.calcTotal()
        },
        //合计行
        calcTotal(){
            var total_price=0.00   //金额（不含税） 
            var total_tax=0.00     //税额
            var total_Tprice=0.00  //价税合计

            this.total_details.forEach(element => {
                total_Tprice+=element.Sumprice == ''? 0 : parseFloat(element.Sumprice)
                total_tax+=element.Tax == ''? 0 : parseFloat(element.Tax)
            }); 
            total_price = total_Tprice - total_tax

            // console.log(total_price)
            if(total_price != NaN){
                this.totalSumPrice=total_price.toFixed(2)
                this.totalTax=total_tax.toFixed(2)
                this.totalTaxPriceBig = total_Tprice.toFixed(2)==0.00 ? '零圆整' : this.money(total_Tprice.toFixed(2))
                this.totalTaxPriceSmall=total_Tprice.toFixed(2)
            } 
        },
        handleAdd(){
            var arr={
                    check:false,
                    Name:'',
                    Type:'',
                    Unit:'',
                    number:'',
                    Unitprice:'',
                    Sumprice:'',
                    Taxrate:'0%',
                    Tax:'',
                    discount:false}
            this.total_details.push(arr)
            this.calcTotal()
        },
        handleDel(){
            let target=this.total_details
            for(var i=0;i<target.length;i++){
                if(target[i].check){
                    target.splice(i,1);
                    i=i-1
                }
            }
            this.calcTotal() 
        },
        addDiscount(){ //显示一行折扣行（总折扣）
            let target=this.total_details

            if(target[target.length-1].discount==false){
                 var arr={
                    check:false,
                    Name:'折扣行',
                    Type:'',
                    Unit:'',
                    number:'',
                    Unitprice:'',
                    Sumprice:'',
                    Taxrate:target[target.length-1].Taxrate,
                    Tax:'',
                    discount:true}
                this.total_details.push(arr)
                }else{ }
            },  
        resetBtn(){
            this.refresh()
            this.calcTotal()
        },
        handleMakeOut (){
            this.mekeOutLoading=true
            setTimeout(() => {
                this.dialogVisible = false
                this.mekeOutLoading=false
                setTimeout(() => {
                    this.dialogSuccessVisible=true
                }, 300);
              }, 2000);
        },
        viewInvoice(){
            this.$message('查看发票详情');
        },
        selectCommodityHandle(index){
            this.currentIndex=index
            this.selectCommodityVisible=true
            this.$nextTick(() => {
                this.$refs.selectCommodity.init();
            })
        },
        getSelectedData(data){
            this.total_details[this.currentIndex].Name=data[0].name
            this.total_details[this.currentIndex].Taxrate=data[0].taxRate

            this.calcPrice(this.currentIndex)
        },
        //金额大写
        money(n){
            if (!/^(0|[1-9]\d*)(\.\d+)?$/.test(n)){
                return ''
            }
             
            var unit="仟佰拾亿仟佰拾万仟佰拾圆角分",str="";
                n += "00";
            var p= n.indexOf('.');
            if(p >=0)
                n=n.substring(0,p)+n.substr(p+1,2);

                unit=unit.substr(unit.length-n.length);

                for(var i=0; i<n.length; i++)
                str +='零壹贰叁肆伍陆柒捌玖'.charAt(n.charAt(i))+unit.charAt(i);
                return str.replace(/零(仟|佰|拾|角)/g,"零").replace(/(零)+/g,"零").replace(/零(万|亿|圆)/g,"$1").replace(/(亿)万|壹(拾)/g, "$1$2").replace(/^圆零?|零分/g,"").replace(/圆$/g,"圆整");
        }
    }
}
</script>

<style lang="scss">
  .makeout-bill-success{
      width:440px;
      height:240px;
      text-align: center;
    }
  .manual-bill{
      width:100%;
  }
  .manual-bill-title{
      font-size: 18px;
      float: left;
      margin-top: 20px;
      margin-left: 24px;
  }
  .manual-btn-card{
      position: fixed;
      right: 0px;
      border-top-left-radius: 0px;
      border-top-right-radius: 0px;
      border-bottom-left-radius: 12px;
      border-bottom-right-radius: 12px;
      margin-right: 20px;  
      height:70px;
      background:rgba(255,255,255,0.6);
      border-radius:0px 0px 12px 12px;
      border:1px solid rgba(217,217,217,1);
      display: flex;
      z-index: 999;
  }
  .add-btn{
      margin-left: 34px;
      margin-top: 17px;
      width:80px;
      height:32px;
      padding-top: 6px;
      padding-left:12px;
      background:rgba(255,255,255,1);
      border-radius:4px;
      border:1px solid rgba(199,199,199,1);
      font-family:PingFangSC-Medium;
      font-weight:500;
      color:rgba(70,197,162,1);
      line-height:20px;
  }
  .dele-btn{
      margin-left: 10px;
      margin-top: 17px;
      width:80px;
      height:32px;
      padding-top: 6px;
      padding-left:12px;
      background:rgba(255,255,255,1);
      border:1px solid rgba(199,199,199,1);
      font-family:PingFangSC-Medium;
      font-weight:500;
      color:rgba(245,34,45,1);
      line-height:20px;
  }
  .discount-btn{
      margin-left: 10px;
      margin-top: 17px;
      width:80px;
      height:32px;
      padding-top: 6px;
      background:rgba(255,255,255,1);
      border-radius:4px;
      border:1px solid rgba(199,199,199,1);
      font-family:PingFangSC-Medium;
      font-weight:500;
      color:rgba(89,89,89,1);
      line-height:20px;
  }
  .reset-btn{
      margin-left: 10px;
      margin-top: 17px;
      width:80px;
      height:32px;
      padding-top: 6px;
      background:rgba(255,255,255,1);
      border-radius:4px;
      border:1px solid rgba(199,199,199,1);
      font-family:PingFangSC-Medium;
      font-weight:500;
      color:rgba(89,89,89,1);
      line-height:20px;
  }
  .make-bill-btn{
      margin-right: 32px;
      margin-left: 10px;
      margin-top: 17px;
      width:100px;
      height:32px;
      padding-top: 6px;
      background:rgba(24,144,255,1);
      border-radius:4px;
      font-family:PingFangSC-Medium;
      font-weight:500;
      color:rgba(255,255,255,1);
      line-height:20px;
      border:none;
  }
  .manual-bill-body{
      width:1200px;
      height:800px;
      text-align: center;
      box-shadow:2px 6px 6px 0px rgba(0,0,0,0.1);
      background:rgba(255,255,255,0.6);
      border:1px solid rgba(217,217,217,1);
      margin:auto;
      display: flex;
  }
  .manual-bill-body-left{
      width:40px;
      height:100%;
      float:left;
  }
  .manual-bill-body-center{
      width:1050px;
      height:100%;
      margin-left:35px;
      float:left;
  }
  .manual-bill-body-tables{
      height:30px;
      width:300px;
      float:right;
      padding:5px;
  }
  .left-circle{
      width:20px;
      height:20px;
      margin-left:10px;
      margin-top: 30px;
      border-radius:45px;
      background-color:#D8D8D8;
  }
  .right-circle{
      width:20px;
      height:20px;
      margin-left:40px;
      margin-top: 30px;
      border-radius:45px;
      background-color:#D8D8D8;
  }
  .manual-bill-body-title{
      width: 460px;
      height: 120px;
      margin-top:20px;
      transform: translateX(60%);
      background-size:cover;
      background-image: url(~@/assets/img/statistic/billLogo.png);
  }
  .manual-bill-table-top{
      width:1050px;
      height: 150px;
      border-collapse: collapse;
  }
  .manual-bill-table-top, .manual-bill-table-top tr td { 
      border:1px solid #800000;
  }
  .manual-bill-table-top-tr{
      min-height:150px;
  }
  .text-purchaser{
      height: 100%;
      width: 25px;
      font-size: 16px;
      color:#B1743A;
      font-family: PingFangSC-Regular, sans-serif;
  }
  .basic-information-top{
      height: 100%;
      width: 500px;
  }
  .basic-information-top-row{
      height:30px;
  }
  .basic-information-top-input-style{
      height:30px;
      width:200px;
      margin-right: 5px;
      border:0;
      border-bottom: 1px solid #666;
      background-color: transparent;
      font-size: 14px;
  }
  .top-input-style{
      font-size: 14px;
      height:20px;
      width: 100%;
      border:0;
      border-bottom: 1px solid #666;
      background-color: transparent;
      margin-right: 5px;
  }
  .bottom-input-style{
      font-size: 14px;
      height:20px;
      width: 100%;
      border:0;
      background-color: transparent;
      margin-right: 5px;
  }
  input{
    outline: none;
  }
  .top-label-style{
      width:100%;
      height:100%;
      line-height:20px;
      font-size: 16px;
      font-family: PingFangSC-Regular, sans-serif;
      color:#B1743A;
  }
  .el-col.last-col{
    padding-right:10px;
  }
  .manual-bill-table-detail-layout{
      width:1050px;
      height:200px;
      border:1px solid #800000;
      border-top:0px;
      overflow-y:scroll;
      overflow-x:hidden
  }
  .manual-bill-table-detail{
      width:100%;
      border-collapse: collapse;
  }
  .manual-bill-table-detail, .manual-bill-table-detail tr td { 
      border:1px solid #800000;
      border-top: 0px;
  }
  .detail-input-style{
      height:25px;
      width: 100%;
      border:0;
      background-color: transparent;
      margin-right: 5px;
      text-align: left;
      font-size:12px;
  }
  .detail-label-style{
      width:100px;
      font-size:16px;
      font-family: PingFangSC-Regular, sans-serif;
      color:#B1743A;
  }
  .detail-label-style-td3{
       width:350px;
       font-size:16px;
       font-family: PingFangSC-Regular, sans-serif;
       color:#B1743A;
  }
  .detail-label-style-td1{
       display:inline;
       border:0;
       background-color: transparent;
       margin-right: 10px;
       text-align: left;
       font-size:12px;
  }
  .table-detail-style-tr1{
      width:100%;
      height:30px;
  }
  .detail-label-style-td2{
      display:inline;  
  }
  .select-input{
      width:80%;
      border:0;
      background-color:transparent;
      font-size:12px;
  }
  .table-detail-style-tr2{
      width:100%;
      height:35px;
  }
  .manual-bill-table-bottom{
      width:1050px;
      height:75px;
      border-top:0px;
      border-bottom:0px;
      border-collapse: collapse;
  }
  .manual-bill-table-bottom, .manual-bill-table-bottom tr td { 
      border:1px solid #800000;
      border-top: 0px;
  }
  .table-bottom-tr1{
      width: 100%;
      height:30px;
  }
  .table-bottom-total{
      width:350px;
      height:100%;
      line-height:20px;
      font-family: PingFangSC-Regular, sans-serif;
      font-size:16px;
      color:#B1743A;
  }
  .table-bottom-totalnumber{
      width:700px;
      height:30px;
      padding:5px;
  }
  .table-bottom-text{
     line-height:20px;
     font-size:16px;
     text-align:right;
  }
 .table-bottom-tr2{
      width: 100%;
      height:45px;
  }
 .manual-bill-table-seller{
      width:1050px;
      height: 150px;
      border-collapse: collapse;
  }
  .manual-bill-table-seller, .manual-bill-table-seller tr td { 
      border:1px solid #800000;
      border-top: 0px;
  }
  .information_note{
      -webkit-appearance: none;
      /* box-shadow:0px 0px 0px rgba(0,0,0,0); */
  }
  .information_note{
      height:98%;
      width:100%;
      border-radius: 4px;
      outline: none;
      background-color:transparent;
      resize: none!important;
      padding:10px;
  }
  .manual-bill-table-bottom-label{
      height:50px;
      width:1050px;
      padding-top:10px;
  }

  .makeout-date-piker{
        width:100%!important;
        margin-top:-4px!important;
        input{
            height:24px!important;
            line-height:24px!important;
            border:0;
            border-bottom: 1px solid #999;
            border-radius:0;
        }
        .el-input__prefix{
            right:5px;
            left:120px;
        }
        .el-input__suffix{
            display: none;
        }
        .el-input__icon{
            line-height: 24px!important;
        }    
    } 
  .makeout--dialog{
        text-align:left;
        .el-dialog__header{
            font-size:18px;
            i {
                margin-right: 10px;
            }
        }
        .el-dialog__body{
            padding-top: 5px;
        }
    }
  .makeout-ok--dialog{
        .el-dialog__body{
            text-align: center!important;
            padding-top: 5px;
            padding-bottom: 10px;
        }
        .dialog-icon {
            color:#67c23a;
            font-size: 64px;
            width:64px;
            height:84px;
        }
        .dialog-info{
            height:28px;
            line-height: 28px;
            font-size:20px;
            font-family:PingFangSC-Medium;
            font-weight:550;
            color:rgba(0,0,0,0.85);
        }
    }
    .loading--dialog{
        .el-dialog__body{
            padding: 18px 10px;
        }
        .el-dialog__header,.el-dialog__footer{
            height:0;
            padding: 0;
        }
        .loading{
            width:52px;
            height:52px;
            display: inline-block;
            background-image: url(~@/assets/img/loading.png);
            background-size:100% 100%;
            background-repeat:no-repeat;
        }
    }
 
   @media screen and (max-width: 1600px){
    .manual-bill-title{
        font-size: 13px;
        margin-top: 14px;
        margin-left: 17px;
    }
    .manual-btn-card{
        position: fixed;
        right: 0px;
        margin-right: 20px;  
        height:48px;
        background:rgba(255,255,255,0.6);
        border-radius:0px 0px 9px 9px;
        border:1px solid rgba(217,217,217,1);
        display: flex;
    }
    .add-btn{
      margin-left: 24px;
      margin-top: 12px;
      width:61px;
      height:23px;
      border-radius:3px;
      font-size:10px;
      line-height:14px;
      padding-top: 4px;
      padding-left:10px;
  }
  .dele-btn{
      margin-left: 7px;
      margin-top: 12px;
      width:61px;
      height:23px;
      border-radius:3px;
      font-size:10px;
      line-height:14px;
      padding-top: 4px;
      padding-left:10px;
  }
  .discount-btn{
      margin-left: 7px;
      margin-top: 12px;
      width:51px;
      height:23px;
      border-radius:3px;
      font-size:10px;
      line-height:14px;
      padding-top: 4px;
      padding-left:11px;
  }
  .reset-btn{
      margin-left: 7px;
      margin-top: 12px;
      width:51px;
      height:23px;
      border-radius:3px;
      font-size:10px;
      line-height:14px;
      padding-top: 4px;
      padding-left:15px;
  }
  .make-bill-btn{
      margin-left: 7px;
      margin-top: 12px;
      margin-right: 24px;
      width:71px;
      height:23px;
      border-radius:3px;
      font-size:10px;
      line-height:14px;
      padding-top: 4px;
      padding-left:16px;
  }
  .manual-bill-body{
      width:860px;
      height:570px;
  }
  .manual-bill-body-left{
      width:28px;
  }
  .left-circle{
      width:15px;
      height:15px;
      margin-top: 20px;
  }
  .right-circle{
      width:15px;
      height:15px;
      margin-left:20px;
      margin-top: 20px;
  }
  .manual-bill-body-center{
      width:752px;
      height:100%;
      margin-left:35px;
      float:left;
  }
  .manual-bill-body-title{
      width: 288px;
      height: 75px;
      margin-top:10px;
      transform: translateX(80%);
  }
  .top-input-style{
      font-size: 11px;
      height:14px;
      margin-right: 3px;
  }
  .manual-bill-table-top{
      width:752px;
      height: 106px;
  }
  .manual-bill-table-top-tr{
      min-height:106px;
  }
  .text-purchaser{
      width: 17px;
      font-size: 12px;
  }
  .basic-information-top{
      width: 359px;
  }
  .basic-information-top-row{
      height:21px;
  }
  .top-label-style{
      font-size: 12px;
  }
  .basic-information-top-input-style{
      height:21px;
      width:143px;
      margin-right: 2px;
  }
  .manual-bill-table-detail-layout{
      width:752px;
      height:142px;
  }
  .detail-label-style{
      font-size:12px;
  }
  .table-detail-style-tr1{
      height:21px;
  }
  .detail-label-style-td1{
       width:17px;
  }
  .detail-label-style-td2{
      font-size:12px;
  }
  .select-input{
      width:70%;
  }
  .table-detail-style-tr2{
      height:25px;
  }
  .manual-bill-table-bottom{
      width:752px;
      height:53px;
  }
  .table-bottom-tr1{
      height:21px;
  }
  .table-bottom-tr2{
      height:32px;
  }
  .table-bottom-total{
      width:222px;
      font-size:12px;
      height:100%;
  }
  .table-bottom-totalnumber{
      width:530px;
      height:100%;
  }
  .table-bottom-text{
     font-size:12px;
  }
  .manual-bill-table-seller{
      width:752px;
      height: 106px;
  }
  .manual-bill-table-bottom-label{
      height:35px;
      width:752px;
      padding-top:5px;
  }
} 
</style>