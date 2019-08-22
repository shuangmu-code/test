<template>
    <div >
        <h2 class="text_info">发票详情</h2>
        <div class="card_content">
          <label class="h3_font">{{company}}</label>
          <hr class="HR" />
          <label class="lable_1">开票时间</label>
          <label class="lable_2">商品名称</label>
          <label class="lable_3">商品金额</label>
          <label class="lable_4">商品税率</label>
          <label class="lable_5">商品税额</label>
          <label class="lable_6">价税合计</label>
          <label class="lable_7">{{date | dateformat}}</label>
          <label class="lable_8">{{name}}</label>
          <label class="lable_9">{{price}}</label>
          <label class="lable_10">{{rate}}</label>
          <label class="lable_11">{{tax}}</label>
          <label class="lable_12">{{total}}</label>
        </div>
       <el-button class="PDF_download" type="primary" @click="PDF_Download()">PDF下载</el-button>
       <el-button class="back" type="primary" @click="Back()">返回</el-button> 
    </div>
</template>

<script>
import { METHODS } from 'http';
export default {
    data(){
        return{
            fpid:this.$route.params.id,
            company:'国药控股湖北有限公司',
            date:'2019-07-04',
            name:'经营租赁/停车费',
            price:'8.57',
            rate:'0.05',
            tax:'0.43',
            total:'9.00',

        }
    },
    mounted: function() {
        console.log(this.fpid);
        this.$http({
            url: this.$http.adornUrl('/invoice/record/info/'+ this.fpid),
            method: 'get',
            params: this.$http.adornParams()
        }).then(({data}) => {
            if (data && data.code === 0) {

                this.company=data.mb.gsmc
                this.date=data.mb.kprq
                this.name=data.mb.ywlx
                this.tax=data.mb.hjse
            }
        })
    },
    filters:{
      dateformat: function (time){
        let date = new Date(time)
        return date.getFullYear() + "-" + (date.getMonth() + 1) + "-" + date.getDate()
      }
    },
    methods:{
     PDF_Download:function(event){
       this.$router.replace({name:'download_invoice'})
       if (event) {
          alert(event.target.tagName)
        }
     },
     Back:function(event){
        if (window.history.length <= 1) {
            this.$router.push({path:'/'})
            return false
        } else {
            this.$router.go(-1)
        }
     },

    }
    
}
</script>


<style lang="scss" scoped>
.text_info{
    width:100px;
    height:25px;
    font-size:18px;
    font-family:PingFangSC-Regular;
    font-weight:400;
    color:rgba(39,39,39,1);
    line-height:25px;
}
.card_content{
position:absolute;
top: 71px;
left: 57px;
width: 554px;
height: 367px;
background-image: url(~@/assets/img/Group3.png);
background-size: cover;
}
.h3_font{
position: relative;
top: 43px;
left: 187px;
width:180px;
height:25px;
font-size:18px;
font-family:PingFangSC-Regular;
font-weight:400;
color:rgba(39,39,39,1);
line-height:25px;
}

.HR{
    border-top:1px rgba(0,0,0,.25) dashed;
    position:relative;
    top:60px;
    width:460px;
}
.lable_1{
position: absolute;
top:120px;
left:87px;
width:100px;
height:20px;
font-size:14px;
font-family:PingFangSC-Regular;
font-weight:400;
color:rgba(140,140,140,1);
line-height:20px;
}
.lable_2{
position: absolute;
top: 155px;
left: 87px;
width:100px;
height:20px;
font-size:14px;
font-family:PingFangSC-Regular;
font-weight:400;
color:rgba(140,140,140,1);
line-height:20px;
}
.lable_3{
position: absolute;
top: 190px;
left: 87px;
width:100px;
height:20px;
font-size:14px;
font-family:PingFangSC-Regular;
font-weight:400;
color:rgba(140,140,140,1);
line-height:20px;
}
.lable_4{
position: absolute;
top:225px ;
left:87px ;
width:100px;
height:20px;
font-size:14px;
font-family:PingFangSC-Regular;
font-weight:400;
color:rgba(140,140,140,1);
line-height:20px;
}
.lable_5{
position: absolute;
top:260px ;
left:87px;
width:100px;
height:20px;
font-size:14px;
font-family:PingFangSC-Regular;
font-weight:400;
color:rgba(140,140,140,1);
line-height:20px;
}
.lable_6{
position: absolute;
top:295px ;
left:87px ;
width:100px;
height:20px;
font-size:14px;
font-family:PingFangSC-Regular;
font-weight:400;
color:rgba(140,140,140,1);
line-height:20px;
}

.lable_7{
    position:absolute;
    top:120px;
    left:238px;
    width:200px;
    height:20px;
    font-size:14px;
    text-align:right;
    font-family:PingFangSC-Regular;
    font-weight:400;
    color:rgba(39,39,39,1);
    line-height:20px;
}
.lable_8{
    position:absolute;
    top:155px;
    left:238px;
    width:200px;
    height:20px;
    font-size:14px;
    text-align:right;
    font-family:PingFangSC-Regular;
    font-weight:400;
    color:rgba(39,39,39,1);
    line-height:20px;
}
.lable_9{
    position:absolute;
    top:190px;
    left:238px;
    width:200px;
    height:20px;
    font-size:14px;
    text-align:right;
    font-family:PingFangSC-Regular;
    font-weight:400;
    color:rgba(39,39,39,1);
    line-height:20px;
}
.lable_10{
    position:absolute;
    top:225px;
    left:238px;
    width:200px;
    height:20px;
    font-size:14px;
    text-align:right;
    font-family:PingFangSC-Regular;
    font-weight:400;
    color:rgba(39,39,39,1);
    line-height:20px;
}
.lable_11{
    position:absolute;
    top:260px;
    left:238px;
    width:200px;
    height:20px;
    font-size:14px;
    text-align:right;
    font-family:PingFangSC-Regular;
    font-weight:400;
    color:rgba(39,39,39,1);
    line-height:20px;
}
.lable_12{
    position:absolute;
    top:295px;
    left:238px;
    width:200px;
    height:20px;
    font-size:14px;
    text-align:right;
    font-family:PingFangSC-Regular;
    font-weight:400;
    color:rgba(39,39,39,1);
    line-height:20px;
}
.PDF_download{
    position:absolute;
    top:450px;
    left:110px;
    width:200px;
    height:40px;
    background:rgba(24,144,255,1);
    border-radius:4px;
}
.back{
    position:absolute;
    top:450px;
    left:350px;
    width:200px;
    height:40px;
    color:rgba(0,0,0,0.65);
    background:rgba(255,255,255,1);
    border-radius:4px;
    border:1px solid rgba(217,217,217,1);
}
@media screen and(max-width:1440px){
    .text_info{
      width:78px;
      height:28px;
      font-size:12px;
    }
    .card_content{
      top: 50px;
      left: 40px;
      width: 360px;
      height: 240px;
     
    }
    .h3_font{
     top: 30px;
     left: 130px;
     width:128px;
     height:18px;
     font-size:12px;
    }
    .HR{
     position:relative;
     top:30px;
     width:300px;
    }
  .lable_1{
     top:75px;
     left:62px;
     width:70px;
     height:14px;
     font-size:10px;
}
    .lable_2{
     top: 100px;
     left: 62px;
     width:70px;
     height:14px;
     font-size:10px;
    }
    .lable_3{
     top: 125px;
     left: 62px;
     width:70px;
     height:14px;
     font-size:10px;
    }
    .lable_4{
     top:150px ;
     left: 62px;
     width:70px;
     height:14px;
     font-size:10px;
    }
    .lable_5{
     top:175px ;
     left: 62px;
     width:70px;
     height:14px;
     font-size:10px;
    }
    .lable_6{
     top:200px ;
     left: 62px;
     width:70px;
     height:14px;
     font-size:10px;
    }
    .lable_7{
     top:75px;
     left:110px;
     width:200px;
     height:14px;
     font-size:10px;
    }
    .lable_8{
     top:100px;
     left:110px;
     width:200px;
     height:14px;
     font-size:10px;
    }
    .lable_9{
     top:125px;
     left:110px;
     width:200px;
     height:14px;
     font-size:10px;
    }
    .lable_10{
     top:150px;
     left:110px;
     width:200px;
     height:14px;
     font-size:10px;
    }
    .lable_11{
     top:175px;
     left:110px;
     width:200px;
     height:14px;
     font-size:10px;
    }
    .lable_12{
     top:200px;
     left:110px;
     width:200px;
     height:14px;
     font-size:10px;
    }
    .PDF_download{
     top:290px;
     left:70px;
     width:140px;
     height:30px;
     }
    .back{
     top:290px;
     left:240px;
     width:140px;
     height:30px;
    }
    
}
@media screen and(max-width: 1100px){
    .text_info{
      width:39px;
      height:14px;
      font-size:6px;
    }
    .card_content{
      top: 25px;
      left: 20px;
      width: 180px;
      height: 120px;
     }
    .h3_font{
     top: 10px;
     left: 60px;
     width:64px;
     height:9px;
     font-size:6px;
    }
    .HR{
     position:relative;
     top:0.1px;
     width:150px;
    }
  .lable_1{
     top:40px;
     left:30px;
     width:35px;
     height:7px;
     font-size:5px;
}
    .lable_2{
     top: 50px;
     left:30px;
     width:35px;
     height:7px;
     font-size:5px;
    }
    .lable_3{
     top: 60px;
     left:30px;
     width:35px;
     height:7px;
     font-size:5px;
    }
    .lable_4{
     top:70px ;
     left:30px;
     width:35px;
     height:7px;
     font-size:5px;
    }
    .lable_5{
     top:80px ;
     left:30px;
     width:35px;
     height:7px;
     font-size:5px;
    }
    .lable_6{
     top:90px ;
     left:30px;
     width:35px;
     height:7px;
     font-size:5px;
    }
    .lable_7{
     top:40px;
     left:55px;
     width:100px;
     height:7px;
     font-size:5px;
    }
    .lable_8{
     top:50px;
     left:55px;
     width:100px;
     height:7px;
     font-size:5px;
    }
    .lable_9{
     top:60px;
     left:55px;
     width:100px;
     height:7px;
     font-size:5px;
    }
    .lable_10{
     top:70px;
     left:55px;
     width:100px;
     height:7px;
     font-size:5px;
    }
    .lable_11{
     top:80px;
     left:55px;
     width:100px;
     height:14px;
     font-size:5px;
    }
    .lable_12{
     top:90px;
     left:55px;
     width:100px;
     height:7px;
     font-size:5px;
    }
    .PDF_download{
     top:145px;
     left:35px;
     width:60px;
     height:15px;
     font-size:5px;
     padding: 0px;

     }
    .back{
     top:145px;
     left:120px;
     width:60px;
     height:15px;
     font-size:5px;
     padding: 0px;
    }
    
}
</style>



