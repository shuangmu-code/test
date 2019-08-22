<template>
    <div class="statistics-page">
        <el-row :gutter="5" class="row-board">
            <el-col :span="6">
                    <div class="data-board board-remain">
                        <el-row :gutter="0">
                            <el-col :span="15" class="inner-col1">
                                <div class="data-num">{{dataBoard1}}</div>
                                <div class="data-describ">发票剩余张数</div>
                            </el-col>
                            <el-col :span="9">
                                <div class="describ-pic s-describ-remain"></div>
                            </el-col>
                        </el-row>
                    </div>
            </el-col>
            <el-col :span="6">
                    <div class="data-board board-total">
                        <el-row :gutter="0">
                            <el-col :span="15" class="inner-col1">
                                <div class="data-num">{{dataBoard2}}</div>
                                <div class="data-describ">发票开具份数</div>
                            </el-col>
                            <el-col :span="9">
                                <div class="describ-pic s-describ-total"></div>
                            </el-col>
                        </el-row>
                    </div>
            </el-col>
            <el-col :span="6">
                    <div class="data-board board-chongHong">
                        <el-row :gutter="0">
                            <el-col :span="15" class="inner-col1">
                                <div class="data-num">{{dataBoard3 | MoneyFormat}}</div>
                                <div class="data-describ">发票冲红金额</div>
                            </el-col>
                            <el-col :span="9">
                                <div class="describ-pic s-describ-chongHong"></div>
                            </el-col>
                        </el-row>
                    </div>
            </el-col>
            <el-col :span="6">
                    <div class="data-board board-sum">
                        <el-row :gutter="0">
                            <el-col :span="15" class="inner-col1">
                                <div class="data-num">{{dataBoard4 | MoneyFormat}}</div>
                                <div class="data-describ">发票总金额</div>
                            </el-col>
                            <el-col :span="9">
                                <div class="describ-pic s-describ-sum"></div>
                            </el-col>
                        </el-row>
                    </div>
            </el-col>
        </el-row>
        
        <el-row :gutter="20">
            <el-col :span="12">
                <el-card>
                    <div slot="header" class="clearfix">
                        <span>发票开具占比</span>
                    </div>
                    <div id="J_chartPieBox_S_1" class="chart-box"></div>
                </el-card>
            </el-col>
            <el-col :span="12">
                <el-card>
                    <div slot="header" class="clearfix">
                        <span>开票业务类型占比</span>
                    </div>
                    <div id="J_chartPieBox_S_2" class="chart-box"></div>
                </el-card>
            </el-col>
        </el-row>

        <el-row>
            <el-col :span="24">
                <el-card>
                    <div slot="header" class="clearfix">
                        <el-col :span="16">
                            <span>开票数量统计</span>
                            <div class="choose-Range">
                                <el-radio-group v-model="radio_range" size="mini" @change="changeTimeRange" fill="#1890FF">
                                <el-radio-button label="全年" border="true"></el-radio-button>
                                <el-radio-button label="本月" border="true"></el-radio-button>
                                </el-radio-group>
                            </div>
                        </el-col>
                        <el-col :span="8">
                            <span>全年开票业务类型</span>
                        </el-col>
                    </div>
                    <el-row>
                        <el-col :span="16">
                            <div id="J_chartBox_S_3" class="chart-box"></div>
                        </el-col>
                        <el-col :span="8">
                            <div id="J_chartPieBox_S_4" class="chart-box"></div>
                        </el-col>
                    </el-row>
                </el-card>
            </el-col>
        </el-row>
    </div>
</template>

<script>
  import echarts from 'echarts'
  export default {
    data () {
      return {
        dataBoard1:'1472',
        dataBoard2:'420',
        dataBoard3:'0',
        dataBoard4:'6752.07',

        chartPie1:null,
        chartPie2:null,
        chartBar:null,
        chartPie4:null,
        Pie1data:[
                        { value: 340, name: '个人开票' },
                        { value: 80, name: '公司开票' }
                    ],
        Pie2data:[
                        { value: 3038.43, name: '停车费' },
                        { value: 2228.00, name: '物业费' },
                        { value: 1485.64, name: '水电费' }
                    ],
        BarTotalData:[
                        { value: 256, name: '1月' },
                        { value: 603, name: '2月' },
                        { value: 842, name: '3月' },
                        { value: 459, name: '4月' },
                        { value: 245, name: '5月' },
                        { value: 356, name: '6月' },
                        { value: 124, name: '7月' },
                        { value: 56, name: '8月' },
                        { value: 356, name: '9月' },
                        { value: 490, name: '10月' },
                        { value: 348, name: '11月' },
                        { value: 85, name: '12月' }
                    ],
        BarYearData:[
                        { value: 256, name: '1月' },
                        { value: 603, name: '2月' },
                        { value: 842, name: '3月' },
                        { value: 459, name: '4月' },
                        { value: 245, name: '5月' },
                        { value: 356, name: '6月' },
                        { value: 124, name: '7月' },
                        { value: 56, name: '8月' },
                        { value: 356, name: '9月' },
                        { value: 490, name: '10月' },
                        { value: 348, name: '11月' },
                        { value: 85, name: '12月' }
                    ],
        BarMonthData:[
                        { value: 25, name: '1' },
                        { value: 63, name: '2' },
                        { value: 82, name: '3' },
                        { value: 49, name: '4' },
                        { value: 25, name: '5' },
                        { value: 36, name: '6' },
                        { value: 14, name: '7' },
                        { value: 6, name: '8' },
                        { value: 36, name: '9' },
                        { value: 10, name: '10' },
                        { value: 38, name: '11' },
                        { value: 8, name: '12' },
                        { value: 8, name: '13' },
                        { value: 8, name: '14' },
                        { value: 8, name: '15' },
                        { value: 8, name: '16' },
                        { value: 8, name: '17' },
                        { value: 8, name: '18' },
                        { value: 8, name: '19' },
                        { value: 8, name: '20' },
                        { value: 8, name: '21' },
                        { value: 8, name: '22' },
                        { value: 8, name: '23' },
                        { value: 8, name: '24' },
                        { value: 8, name: '25' },
                        { value: 8, name: '26' },
                        { value: 8, name: '27' },
                        { value: 8, name: '28' },
                        { value: 8, name: '29' },
                        { value: 8, name: '30' },
                        { value: 8, name: '31' },
                    ],
        Pie4Data:[
                        { value: 2200, name: '停车费' },
                        { value: 6356, name: '物业费' },
                        { value: 1958, name: '水电费' },
                        { value: 1712, name: '其他' }
                    ],
        radio_range:'全年'
      }
    },
    filters: {
        MoneyFormat: function (value) {
            if (!value) return '0.00';

            value=value.toString()
            
			/*原来用的是Number(value).toFixed(0)，这样取整时有问题，例如0.51取整之后为1，感谢Nils指正*/
			var intPart =  Number(value)|0; //获取整数部分
			var intPartFormat = intPart.toString().replace(/(\d)(?=(?:\d{3})+$)/g, '$1,'); //将整数部分逢三一断

			var floatPart = ".00"; //预定义小数部分
			var value2Array = value.split(".");

			//=2表示数据有小数位
			if(value2Array.length == 2) {
				floatPart = value2Array[1].toString(); //拿到小数部分

				if(floatPart.length == 1) { //补0,实际上用不着
					return '￥'+intPartFormat + "." + floatPart + '0';
				} else {
					return '￥'+intPartFormat + "." + floatPart;
				}

			} else
				return '￥'+intPartFormat + floatPart;
        }
    },
    mounted(){

        this.initChartPie1()
        this.initChartPie2()
        this.initChartBar()
        this.initChartPie4()
    },
    activated(){
         // 由于给echart添加了resize事件, 在组件激活时需要重新resize绘画一次, 否则出现空白bug
        if (this.chartPie1) {
            this.chartPie1.resize()
        }
        if (this.chartPie2) {
            this.chartPie2.resize()
        }
        if (this.chartBar) {
            this.chartBar.resize()
        }
        if (this.chartPie4) {
            this.chartPie4.resize()
        }
    },
    methods: {
        setFontSize(res){
            let docEl = document.documentElement,
                clientWidth = window.innerWidth||document.documentElement.clientWidth||document.body.clientWidth;
            if (!clientWidth) return;
            let fontSize = 100 * (clientWidth / 1920);
            return res*fontSize;
        },
        initChartPie1 () {
            const _dataList=this.Pie1data;
            let _num=0,_flag=true;
            _dataList.forEach((item,i)=>{
                _num+=item.value
            })
            var option = {
                color: ['#FFA65A','#5244FF'],
                tooltip: {
                    trigger: 'item',
                    formatter: '{b} : {c} ({d}%)'
                },
                legend: {
                    orient: 'vertical',
                    top:"center",
                    left:'50%',
                    data:[
                        {
                            name:'个人开票',
                            textStyle:{color:'#FFA65A'}
                        },
                        {
                            name:'公司开票',
                            textStyle:{color:'#5244FF'}
                        },
                    ],
                    itemWidth:24,
                    itemHeight:8,
                    itemGap:20,
                    formatter:function(name){
                        let _index=0;
                        let _total=0;
                        let _percent=0;
                        _dataList.forEach((item,i)=>{
                            _total+=item.value
                            if(item.name == name){
                            _index = i;
                            }
                        })
                        if(_total!=0)
                            _percent=Number((_dataList[_index].value/_total)*100).toFixed(2)
                            _percent=Math.round(_percent)
                        return "{title|"+name+"}\r\t\r\t{value|"+_dataList[_index].value+"\r\t\r\t"+_percent+"%}"
                    },
                    textStyle:{
                        rich:{
                            title:{
                                fontSize:this.setFontSize(0.16),
                                color:"rgba(0,0,0,.45)"
                            },
                            value:{
                                fontSize:this.setFontSize(0.16),
                                fontWeight:600,
                            }
                        }
                    }
                },
                series: [
                    {
                        type: 'pie',
                        radius: ['50%',"75%"],
                        data: this.Pie1data,
                        center: ['20%', '50%'],
                        label:{
                            normal:{
                                show:false
                            }
                        },
                        labelLine: {
                            normal: {
                                show: false
                            }
                        },
                    },
                    {
                        name:'Mask',
                        type:'pie',
                        radius : ['25%'],
                        center : ['20%', '50%'],
                        label: {
                            normal: {
                                show: true,
                                position:'center',
                                formatter:'{title|总数}\n\n{value|'+_num+'}',
                                textStyle:{
                                    rich:{
                                        title:{
                                            fontSize:11,
                                            color:"rgba(0,0,0,.45)"
                                        },
                                        value:{
                                            fontSize:14,
                                            fontWeight:600,
                                            color:'#272727'
                                        }
                                    }
                                }
                            },
                            emphasis: {
                                show: true
                            }
                        },
                        lableLine: {
                            normal: {
                                show: false
                            },
                            emphasis: {
                                show: false
                            }
                        },
                        data: [{
                            value: _num,
                            name: '总数',
                            itemStyle: {
                                normal: {
                                    color:'rgba(255,255,255,.45)',
                                }
                            }
                        }]
                    }
                ]
            }
            this.chartPie1 = echarts.init(document.getElementById('J_chartPieBox_S_1'))
            this.chartPie1.setOption(option)
            window.addEventListener('resize', () => {
                this.chartPie1.resize()
            })
        },
        initChartPie2(){
            const _dataList=this.Pie2data;
            var _dataName=[]
            _dataList.forEach((item,i)=>{
                            _dataName[i]=item['name'] })
            console.log(_dataName);

            //使用this.$options.filters[]方式获取本地过滤器
            var te = this.$options.filters['MoneyFormat'];

            var option = {
                color:['#4BBCFE','#FEA255','#FB526D'],
                tooltip : {
                    trigger: 'item',
                    formatter: "{a} <br/>{b} : {c} ({d}%)"
                },
                legend: {
                    orient: 'vertical',
                    top:"center",
                    left:'50%',
                    icon: "roundRect",
                    data:_dataName,
                    itemWidth:24,
                    itemHeight:8,
                    itemGap:16,
                    formatter:function(name){
                        let _index=0;
                        let _total=0;
                        let _percent=0;
                        _dataList.forEach((item,i)=>{
                            _total+=item.value
                            if(item.name == name){
                            _index = i;
                            }
                        })
                        if(_total!=0)
                            _percent=Number((_dataList[_index].value/_total)*100).toFixed(2)
                            _percent=Math.round(_percent)
                        return "{title|"+name+"}\t\t {value|"+te(_dataList[_index].value)+"\t\t"+_percent+"% }"
                    },
                    textStyle:{
                        rich:{
                            title:{
                                fontSize:this.setFontSize(0.14),
                                color:"rgba(0,0,0,.45)"
                            },
                            value:{
                                fontSize:this.setFontSize(0.14),
                                fontWeight:600,
                                color:"rgba(0,0,0,.85)"
                            }
                        }
                    }
                },
                calculable : true,
                series : [
                    {
                        name:'业务类型',
                        type:'pie',
                        radius : ['15%', '70%'],
                        center : ['25%', '50%'],
                        roseType : 'radius',
                        label: {
                            normal: {
                                show: false
                            },
                            emphasis: {
                                show: true
                            }
                        },
                        lableLine: {
                            normal: {
                                show: false
                            },
                            emphasis: {
                                show: true
                            }
                        },
                        data:this.Pie2data,
                    },
                    {
                        name:'Mask',
                        type:'pie',
                        z:100,
                        radius : ['25%'],
                        center : ['25%', '50%'],
                        label: {
                            normal: {
                                show: false
                            },
                            emphasis: {
                                show: true
                            }
                        },
                        lableLine: {
                            normal: {
                                show: false
                            },
                            emphasis: {
                                show: true
                            }
                        },
                        data: [{
                            value: 100,
                            name: '',
                            itemStyle: {
                                normal: {
                                    color:'rgba(255,255,255,.45)',
                                }
                            }
                        }]
                    }
                ]
            }
            this.chartPie2 = echarts.init(document.getElementById('J_chartPieBox_S_2'))
            this.chartPie2.setOption(option)
            window.addEventListener('resize', () => {
                this.chartPie2.resize()
            })
        },
        initChartBar(){
            const _dataList=this.BarTotalData;
            var _dataName=[]
            _dataList.forEach((item,i)=>{
                _dataName[i]=item['name'] })

            var option = {
                color:['#53A2FF'],
                tooltip: {
                    trigger: 'axis',
                    axisPointer: {
                        type: 'shadow'
                    }
                },
                grid: {
                    left: '3%',
                    top:'5%',
                    right: '12%',
                    bottom: '1%',
                    containLabel: true
                },
                xAxis: {
                    type: 'category',
                    data: _dataName,
                    axisTick:{
			                show:false
                    },
                    axisLine:{
                        lineStyle:{
                            color:'#D9D9D9'
                        }
                    },
                    axisLabel: {
                        show: true,
                        textStyle: {
                            color: 'rgba(0, 0, 0, 0.65)',  //更改坐标轴文字颜色
                            fontSize : 9      //更改坐标轴文字大小
                        }
                    },
                },
                yAxis: {
                    type: 'value',
                    name:'开票数量',
                    splitLine :{
                        lineStyle:{
                            type:'dashed',
                            color:'#E8E8E8'
                        },
                        show:true
                    },
                    axisTick:{
			            show:false
                    },
                    axisLine:{
                        show:false
                    },
                    axisLabel: {
                        show: true,
                        textStyle: {
                            color: 'rgba(0, 0, 0, 0.65)',  //更改坐标轴文字颜色
                            fontSize : 9      //更改坐标轴文字大小
                        }
                    },
                },
                series: [
                    {
                        name: '开票统计',
                        type: 'bar',
                        barWidth:'15',
                        data: _dataList,
                        itemStyle: {
                            //柱形图圆角，鼠标移上去效果
                            emphasis: {
                                barBorderRadius: [2, 2, 0, 0]
                            },
                            
                            normal: {
                                //柱形图圆角，初始化效果
                                barBorderRadius:[2, 2, 0, 0],
                                label: {
                                    color: '#000000',
                                    show: false,
                                    position: 'top', 
                                },
                            }
                        },
                    }
                ]
            }
            this.chartBar = echarts.init(document.getElementById('J_chartBox_S_3'))
            this.chartBar.setOption(option)
            window.addEventListener('resize', () => {
                this.chartBar.resize()
            })
        },
        initChartPie4 () {
            const _dataList=this.Pie4Data;
            let _num=0,_flag=true;
            var _dataName=[]
            _dataList.forEach((item,i)=>{
                _num+=item.value
                _dataName[i]=item['name'] 
            })
            var option = {
                color: ['#FFAA36','#3BA0FF','#F2637B','#36CBCB'],
                tooltip: {
                    trigger: 'item',
                    formatter: '{b} : {c} ({d}%)'
                },
                legend: {
                    orient: 'vertical',
                    top:"center",
                    left:'54%',
                    data:_dataName,
                    itemWidth:24,
                    itemHeight:8,
                    itemGap:12,
                    formatter:function(name){
                        let _index=0;
                        _dataList.forEach((item,i)=>{
                            if(item.name == name){
                            _index = i;
                            }
                        })
                        return "{title|"+name+"}\r\t\r\t{value|"+_dataList[_index].value+"}"
                    },
                    textStyle:{
                        rich:{
                            title:{
                                fontSize:11,
                                color:"rgba(0,0,0,.45)"
                            },
                            value:{
                                fontSize:12,
                                fontWeight:600,
                            }
                        }
                    }
                },
                series: [
                    {
                        type: 'pie',
                        radius: ['50%',"75%"],
                        data: this.Pie4Data,
                        center: ['30%', '50%'],
                        label:{
                            normal:{
                                show:false
                            }
                        },
                        labelLine: {
                            normal: {
                                show: false
                            }
                        },
                    },
                    {
                        name:'Mask',
                        type:'pie',
                        radius : ['25%'],
                        center : ['30%', '50%'],
                        label: {
                            normal: {
                                show: true,
                                position:'center',
                                formatter:'{title|总数}\n\n{value|'+_num+'}',
                                textStyle:{
                                    rich:{
                                        title:{
                                            fontSize:11,
                                            color:"rgba(0,0,0,.45)"
                                        },
                                        value:{
                                            fontSize:14,
                                            fontWeight:600,
                                            color:'#272727'
                                        }
                                    }
                                }
                            },
                            emphasis: {
                                show: true
                            }
                        },
                        lableLine: {
                            normal: {
                                show: false
                            },
                            emphasis: {
                                show: false
                            }
                        },
                        data: [{
                            value: _num,
                            name: '总数',
                            itemStyle: {
                                normal: {
                                    color:'rgba(255,255,255,.45)',
                                }
                            }
                        }]
                    }
                ]
            }
            this.chartPie4 = echarts.init(document.getElementById('J_chartPieBox_S_4'))
            this.chartPie4.setOption(option)
            window.addEventListener('resize', () => {
                this.chartPie4.resize()
            })
        },
        changeTimeRange(){
            console.log('Time_range change')
            if(this.radio_range=="全年")
                this.BarTotalData=this.BarYearData
            else
                this.BarTotalData=this.BarMonthData
            this.initChartBar()

        }
    }
  }
</script>

<style scoped>
  .el-row {
    margin-bottom: 20px;
  }
  .row-bg {
    padding: 10px 0;
    background-color: #f9fafc;
  }
  .row-board{
    height: 113px;
  }
  .row-board.el-col{
    border-radius: 4px;
  }
  /*.el-card__header{
      padding: 24px 20px 5px 20px;
      border-bottom: 0;
  }*/
  .data-board {
    height:200px;
    border-radius: 4px;
    padding: 20px 15px;
  }
  .data-board .el-row{
      margin-bottom: 0;
  }
  .inner-col1{
    text-align:left;
    padding-left: 38px;
  }
  .data-num{
    margin-top:35px;
    font-size:40px;
    font-family:SFUIText-Medium;
    font-weight:500;
    color:#272727;
    line-height:48px;
  }
  .data-describ{
    color:#8C8C8C;
    font-size:18px;
    font-family:PingFangSC-Regular;
    margin-top: 28px;
    letter-spacing:1px;
  }
  .describ-pic{
    width:100px;
    height:68px;
    margin-top:66px;
  }
  .describ-pic.s-describ-sum{
    width:80px;
    height:80px;
    margin-left:12px;
    margin-top: 54px;
  }
  .board-remain,.board-total,.board-chongHong,.board-sum{
    background-image: url(~@/assets/img/statistic/Mask.png);
    background-size:100% 100%;
	background-repeat:no-repeat;
  }
  .s-describ-remain{
    background-image: url(~@/assets/img/statistic/dataBoard1.png);
    background-size: cover;
  }
  .s-describ-total{
    background-image: url(~@/assets/img/statistic/dataBoard2.png);
    background-size: cover;
  }
  .s-describ-chongHong{
    background-image: url(~@/assets/img/statistic/dataBoard3.png);
    background-size: cover;
  }
  .s-describ-sum{
    background-image: url(~@/assets/img/statistic/dataBoard4.png);
    background-size: cover;
  }

  .clearfix{
    font-size: 18px;
    font-weight: 600;
    font-family:PingFangSC-Medium;
  }
  .chart-box {
    min-height: 200px;
  }
  
  .choose-Range{
    float: right;
    margin-right: 10%;
  }

 @media screen and (max-width: 1500px){
    .inner-col1{
        padding-left: 24px;
    }
    .data-board {
        height:150px;
    }
    .data-num{
        margin-top:16px;
        font-size:30px;
    }
    .data-describ{
        font-size: 14px;
        margin-top: 14px;
    }
    .describ-pic{
        width:75px;
        height:51px;
        margin-top:45px;
    }
    .describ-pic.s-describ-sum{
        width:60px;
        height:60px;
        margin-top: 36px;
    }
    .clearfix{
        font-size: 15px;
    }
    .chart-box {
        min-height: 150px;
    }
}
@media screen and (max-width: 1300px){
    .inner-col1{
        padding-left: 18px;
    }
    .data-board {
        height:133px;
    }
    .data-num{
        margin-top:10px;
        font-size:27px;
    }
    .data-describ{
        font-size: 12px;
        margin-top: 12px;
    }
    .describ-pic{
        width:67px;
        height:45px;
        margin-top:35px;
    }
    .describ-pic.s-describ-sum{
        width:53px;
        height:53px;
        margin-top: 30px;
    }
    .clearfix{
        font-size: 13px;
    }
    .chart-box {
        min-height: 120px;
    }
}
</style>
