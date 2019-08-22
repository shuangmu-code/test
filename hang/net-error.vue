<template>
    <div class="site-info--content">
        <div class="title_pic"></div>
        <div class="title">没有网络</div>
        <div class="sub_title">没有网络，请点击下方按钮刷新页面</div>
        <el-button type="primary" class="reflesh_btn" @click.native="refresh()">刷新</el-button>

        <el-button :plain="true" @click="open_message">打开消息提示</el-button>
        <el-tooltip placement="right-end">
            <div slot="content">{{myword}}</div>
            <span>{{myword | ellipsis}}</span>
        </el-tooltip>
        <div>
          <el-button type="text" @click="open2">点击打开 Message Box</el-button>  
          <el-button type="text" @click="dialogDelVisible = true">删除</el-button>
        </div>

        <el-button type="text" @click="dialogVisible = true">签章开票</el-button>
        <el-button type="text" @click="dialogSuccessVisible = true">开票成功</el-button>
        <el-button type="text" @click="startLoading">全局加载</el-button>

        <el-dialog :visible.sync="dialogDelVisible" width="30%" top="36vh" class="delete--dialog">
            <span slot="title"><i class="el-notification__icon el-icon-error" style="color:#F5222D"></i>删除信息</span>
            <span style="padding-left:30px">确认要删除这条信息吗？</span>
            <span slot="footer" class="dialog-footer">
                <el-button @click="dialogDelVisible = false">取 消</el-button>
                <el-button type="primary" @click="handleDel">
                    确 定
                </el-button>
            </span>
        </el-dialog>

        <el-dialog :visible.sync="dialogVisible" width="30%" top="36vh" :before-close="handleDClose" class="makeout--dialog">
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

        <el-dialog :visible.sync="gloabLoadingVisible" width="20%" top="46vh" :show-close="false" class="loading--dialog">
            <div class="loading"></div>
            <div>正在加载，请稍等</div>
            <span slot="footer" class="dialog-footer"></span>
        </el-dialog>

    </div>
</template>

<script>
export default {
    data(){
        return{
            myword:'购方银行账号购方银行账号购方银行账号购方银行账号购方银行账号购方银行账号购方银行账号',
            dialogDelVisible:false,
            gloabLoadingVisible:false,
            dialogVisible:false,
            dialogSuccessVisible:false,
            mekeOutLoading:false
        }
    },
    filters: {
        ellipsis (value) {
        if (!value) return ''
        if (value.length > 15) {
            return value.slice(0,15) + '...'
        }
        return value
        }
    },
    methods: {
        open_message() {
            this.$message('这是一条消息提示');
        },
        open2() {
            const h = this.$createElement;
            this.$confirm('确认要删除这条信息吗?', '删除信息', {
            confirmButtonText: '确定',
            cancelButtonText: '取消',
            dangerouslyUseHTMLString: true,
            type: 'error'
            }).then(() => {
            this.$message({
                type: 'success',
                message: '删除成功!'
            });
            }).catch(() => {
            this.$message({
                type: 'info',
                message: '已取消删除'
            });          
            });
        },
        handleDel(){
            this.dialogDelVisible=false
            this.$message({
                type: 'success',
                message: '删除成功!'
            });
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
        startLoading(){
            this.gloabLoadingVisible = true
            /*setTimeout(() => {
                this.gloabLoadingVisible = false
            }, 3000);*/
        }
    }
}
</script>

<style lang="scss" scoped>
    .site-info--content{
        text-align: center;
    }
    .title_pic{
        width:254px;
        height: 150px;
        margin: 0 auto;
        margin-top: 170px;
        background: url(~@/assets/img/fault/netError.png);
        background-size:100% 100%;
	    background-repeat:no-repeat;
    }
    .title{
        font-size: 20px;
        font-family:PingFangSC-Regular;
        font-weight:400;
        color:rgba(39,39,39,1);
        height:28px;
        line-height:28px;
    }
    .sub_title{
        height:20px;
        margin-top: 10px;
        font-size:14px;
        font-family:PingFangSC-Regular;
        font-weight:400;
        color:rgba(140,140,140,1);
        line-height:20px;
    }
    .reflesh_btn{
        width:160px;
        height:40px;
        margin-top: 20px;
    }

    /deep/.delete--dialog{
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

    /deep/.makeout--dialog{
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
    /deep/.makeout-ok--dialog{
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

    /deep/.loading--dialog{
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
            animation:loading_animate .8s linear infinite;
        }
    }
    @keyframes loading_animate{
 
		0%{-webkit-transform:rotate(0deg);}
 
		25%{-webkit-transform:rotate(90deg);}
 
		50%{-webkit-transform:rotate(180deg);}
 
		75%{-webkit-transform:rotate(270deg);}
 
		100%{-webkit-transform:rotate(360deg);}
 
	}
</style>


