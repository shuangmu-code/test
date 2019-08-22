<template>
    <div class="content">
        <label class="lable">发票详情</label>
        <div class="pdf_iframe">
            <iframe src= "../../static/example.pdf" width='100%' height='768px' frameborder='0'>
            </iframe>
        </div>
     </div>
</template>



<script>

export default {
    data(){
        return{
         url:'../../static/example.pdf'
        
        }
    }
    
}
</script>


<style lang="scss">
.content{
    width:100%;
    min-height: 600px;
    background:rgba(255,255,255,1);
    box-shadow:0px 4px 10px 0px rgba(62,134,247,0.1);
    border-radius:6px;
}
.lable{
    height:25px;
    font-size:18px;
    font-family:PingFangSC-Regular;
    font-weight:400;
    color:rgba(39,39,39,1);
    line-height:25px;
    display: inline-block;
}
.pdf_iframe{
    min-height:600px;

}
</style>

