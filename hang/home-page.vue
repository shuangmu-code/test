<template>
    <div class="home-page">
        <el-row :gutter="5" class="row-board">
            <el-col :span="6">
                    <div class="data-board board-remain">
                        <el-row :gutter="0">
                            <el-col :span="15" class="inner-col1">
                                <span class="data-num">{{dataBoard1}}</span>
                                <div class="data-describ">发票剩余张数</div>
                            </el-col>
                            <el-col :span="9">
                                <div class="describ-pic describ-remain"></div>
                            </el-col>
                        </el-row>
                    </div>
            </el-col>
            <el-col :span="6">
                    <div class="data-board board-total">
                        <el-row :gutter="0">
                            <el-col :span="15" class="inner-col1">
                                <span class="data-num">{{dataBoard2}}</span>
                                <div class="data-describ">发票开具份数</div>
                            </el-col>
                            <el-col :span="9">
                                <div class="describ-pic describ-total"></div>
                            </el-col>
                        </el-row>
                    </div>
            </el-col>
            <el-col :span="6">
                    <div class="data-board board-chongHong">
                        <el-row :gutter="0">
                            <el-col :span="15" class="inner-col1">
                                <span class="data-num">{{dataBoard3 | MoneyFormat}}</span>
                                <div class="data-describ">发票冲红金额</div>
                            </el-col>
                            <el-col :span="9">
                                <div class="describ-pic describ-chongHong"></div>
                            </el-col>
                        </el-row>
                    </div>
            </el-col>
            <el-col :span="6">
                    <div class="data-board board-sum">
                        <el-row :gutter="0">
                            <el-col :span="15" class="inner-col1">
                                <span class="data-num">{{dataBoard4 | MoneyFormat}}</span>
                                <div class="data-describ">发票总金额</div>
                            </el-col>
                            <el-col :span="9">
                                <div class="describ-pic describ-sum"></div>
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
                    <div id="J_chartPieBox1" class="chart-box"></div>
                </el-card>
            </el-col>
            <el-col :span="12">
                <el-card>
                    <div slot="header" class="clearfix">
                        <span>开票业务类型占比</span>
                    </div>
                    <div id="J_chartPieBox2" class="chart-box"></div>
                </el-card>
            </el-col>
        </el-row>

        <el-row>
            <el-col :span="24">
                <el-card>
                    <div slot="header" class="clearfix">
                        <el-col :span="19">
                            <span>开票数量统计</span>
                            <div class="choose-time">
                                <el-radio-group v-model="radio_range" size="mini" @change="changeTimeRange" fill="#1890FF">
                                <el-radio-button label="本周" border="true"></el-radio-button>
                                <el-radio-button label="今日" border="true"></el-radio-button>
                                </el-radio-group>
                            </div>
                        </el-col>
                        <el-col :span="5">
                            <span>开票数排名</span>
                        </el-col>
                    </div>
                    <el-row>
                        <el-col :span="19">
                            <div id="J_chartBox3" class="chart-box"></div>
                        </el-col>
                        <el-col :span="5">
                            <div class="rank-box">
                                
                                <ul>
                                    <li v-for="(item, index) in BarRankData" :key="item.name">
                                        <div v-if="index < 3">
                                            <div class="rank-index rank-top">{{ index+1}}</div>
                                        </div>
                                        <div v-else>
                                            <div class="rank-index">{{ index+1}}</div>
                                        </div>
                                        <span class="rank-name">{{ item.name}}</span>
                                        <span class="rank-value">{{ item.value}}</span> 
                                    </li>
                                </ul>
                            </div>
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
        Pie1data:[
                        { value: 340, name: '个人开票' },
                        { value: 80, name: '公司开票' }
                    ],
        Pie2data:[
                        { value: 3038.43, name: '停车费' },
                        { value: 2228.00, name: '物业费' },
                        { value: 1485.64, name: '水电费' }
                    ],
        BarData:[
                        { value: 203, name: '水电' },
                        { value: 261, name: '停车' },
                        { value: 148, name: '物业' },
                        { value: 85, name: '餐饮' }
                    ],
        BarRankData:[
                        { value: 261, name: '停车' },
                        { value: 203, name: '水电' },
                        { value: 148, name: '物业' },
                        { value: 85, name: '餐饮' },
                        { value: 85, name: '其他' },
                    ],
        radio_range:'本周'
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
    },
    methods: {
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
                    itemWidth:20,
                    itemHeight:5,
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
            this.chartPie1 = echarts.init(document.getElementById('J_chartPieBox1'))
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
                    itemWidth:20,
                    itemHeight:5,
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
                                fontSize:12,
                                color:"rgba(0,0,0,.45)"
                            },
                            value:{
                                fontSize:12,
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
            this.chartPie2 = echarts.init(document.getElementById('J_chartPieBox2'))
            this.chartPie2.setOption(option)
            window.addEventListener('resize', () => {
                this.chartPie2.resize()
            })
        },
        initChartBar(){
            const _dataList=this.BarData;
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
                legend: {
                    
                },
                grid: {
                    left: '3%',
                    top:'3%',
                    right: '16%',
                    bottom: '1%',
                    containLabel: true
                },
                xAxis: {
                    name:'数量(张)',
                    type: 'value',
                    boundaryGap: [0, 0.01],
                    splitLine:{
				        	show:false
                    },
                    axisLabel: {
                        show: true,
                        textStyle: {
                            color: 'rgba(0, 0, 0, 0.65)',  //更改坐标轴文字颜色
                            fontSize : 9      //更改坐标轴文字大小
                        }
                    },
                    axisLine:{
                        lineStyle:{
                            color:'#D9D9D9'
                        }
                    },
                },
                yAxis: {
                    type: 'category',
                    data: _dataName,
                    axisTick:{
			            show:false
                    },
                    axisLabel: {
                        show: true,
                        textStyle: {
                            color: 'rgba(0, 0, 0, 0.65)',  //更改坐标轴文字颜色
                            fontSize : 10     //更改坐标轴文字大小
                        }
                    },
                    axisLine:{
                        lineStyle:{
                            color:'#D9D9D9'
                        }
                    },
                },
                series: [
                    {
                        name: '开票统计',
                        type: 'bar',
                        barWidth:'10',
                        data: _dataList,
                        itemStyle: {
                            //柱形图圆角，鼠标移上去效果
                            emphasis: {
                                barBorderRadius: [0, 10, 10, 0]
                            },
                            
                            normal: {
                                //柱形图圆角，初始化效果
                                barBorderRadius:[0, 10, 10, 0],
                                label: {
                                    color: '#000000',
                                    show: true,
                                    position: 'right', 
                                },
                            }
                        },
                    }
                ]
            }
            this.chartBar = echarts.init(document.getElementById('J_chartBox3'))
            this.chartBar.setOption(option)
            window.addEventListener('resize', () => {
                this.chartBar.resize()
            })
        },
        changeTimeRange(){
            if(this.radio_range=='本周'){
                this.BarData=[
                        { value: 203, name: '水电' },
                        { value: 261, name: '停车' },
                        { value: 148, name: '物业' },
                        { value: 85, name: '餐饮' }
                    ]
                this.BarRankData=[
                        { value: 203, name: '水电' },
                        { value: 261, name: '停车' },
                        { value: 148, name: '物业' },
                        { value: 85, name: '餐饮' },
                        { value: 0, name: '其他' }
                    ]
            }else{
                this.BarData=[
                        { value: 13, name: '水电' },
                        { value: 21, name: '停车' },
                        { value: 8, name: '物业' },
                        { value: 16, name: '餐饮' }
                    ]
                this.BarRankData=[
                        { value: 21, name: '停车' },
                        { value: 16, name: '餐饮' },
                        { value: 13, name: '水电' },
                        { value: 8, name: '物业' },
                        { value: 0, name: '其他' }
                    ]
            }
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
    height:113px;
    border-radius: 4px;
  }
  .el-card__header{
      padding: 24px 20px 5px 20px;
      border-bottom: 0;
      font-size: 14px;
      font-weight: 600;
      font-family:PingFangSC-Medium;
  }
  /*.el-card__body{
    padding: 5px 20px 20px 15px;
  }*/
  .data-board {
    height:100%;
    border-radius: 4px;
    padding: 20px 15px;
  }
  .data-board .el-row{
      margin-bottom: 0;
  }
  .inner-col1{
    text-align:left;
    padding-left: 20px;
  }
  .data-num{
    top:52px;
    left:58px;
    font-size:23px;
    font-family:SFUIText-Medium;
    font-weight:500;
    color:rgba(255,255,255,1);
    line-height:48px;
  }
  .data-describ{
    color:rgba(255, 255, 255, 0.9);
    font-size:10px;
    margin-top: 6px;
  }
  .describ-pic{
    width:56px;
    height:38px;
    margin-top: 40%;
  }
  .describ-pic.describ-sum{
    width:45px;
    height:45px;
    margin-top: 35%;
  }
  .board-remain{
    background-image: url(~@/assets/img/home/dataBoardMask1.png);
    background-size: cover;
  }
  .describ-remain{
    background-image: url(~@/assets/img/home/dataBoard1.png);
    background-size: cover;
  }
  .board-total{
    background-image: url(~@/assets/img/home/dataBoardMask2.png);
    background-size: cover;
  }
  .describ-total{
    background-image: url(~@/assets/img/home/dataBoard2.png);
    background-size: cover;
  }
  .board-chongHong{
    background-image: url(~@/assets/img/home/dataBoardMask3.png);
    background-size: cover;
  }
  .describ-chongHong{
    background-image: url(~@/assets/img/home/dataBoard3.png);
    background-size: cover;
  }
  .board-sum{
    background-image: url(~@/assets/img/home/dataBoardMask4.png);
    background-size: cover;
  }
  .describ-sum{
    background-image: url(~@/assets/img/home/dataBoard4.png);
    background-size: cover;
  }

  .chart-box {
    min-height: 140px;
  }
  
  .choose-time{
    float: right;
    margin-right: 50px;
  }
  .rank-box ul{
    padding: 0;
    width:200px;
  }
  .rank-box ul li{
      list-style: none;
      width:120px;
      line-height: 16px;
      margin-bottom: 10px;
  }
  .rank-index{
    width:16px;
    height:16px;
    line-height: 16px;
    font-size: 12px;
    float: left;
    text-align: center;
    border-radius:8px;
    color:#8F8F8F;
    background-color: #F0F2F5;
  }
  .rank-index.rank-top{
    color:#f9fafc;
    background-color: #1890FF;
  }
  .rank-name{
    color:#8C8C8C;
    margin-left: 10px;
  }
  .rank-value{
    color:rgba(0,0,0,0.85);
    font-weight: 600;
    float: right;
  }

</style>
