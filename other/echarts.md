### Echarts 实例应用

#### 按需引入

```javascript
//echarts按需引入表
require("./lib/component/dataset"); //数据集

require("./lib/chart/line");  //折线

require("./lib/chart/bar");  //柱状图

require("./lib/chart/pie");   //饼图

require("./lib/chart/scatter");  //散点图

require("./lib/chart/radar");  //雷达图

require("./lib/chart/map");  //地图

require("./lib/chart/tree");  //树图

require("./lib/chart/treemap");  //矩形树图

require("./lib/chart/graph");  //关系图

require("./lib/chart/gauge");  //仪表盘

require("./lib/chart/funnel");  //漏斗图

require("./lib/chart/parallel");  //平行坐标

require("./lib/chart/sankey");  //桑基图

require("./lib/chart/boxplot");  //箱线图

require("./lib/chart/candlestick");  //K线图

require("./lib/chart/effectScatter");  //涟漪散点图

require("./lib/chart/lines");  //线图

require("./lib/chart/heatmap");  //热力图

require("./lib/chart/pictorialBar");  //象形柱图

require("./lib/chart/themeRiver");  //主题河流图

require("./lib/chart/sunburst");  //旭日图

require("./lib/chart/custom");  //自定义
//-----------------------------------------------------------
require("./lib/component/grid");  //直角坐标系

require("./lib/component/polar");  //极坐标系

require("./lib/component/geo");  //地理坐标系

require("./lib/component/singleAxis");  //单轴

require("./lib/component/parallel");  //

require("./lib/component/calendar");  //日历

require("./lib/component/graphic");  //

require("./lib/component/toolbox");  //工具栏

require("./lib/component/tooltip");  //提示框

require("./lib/component/axisPointer");  //

require("./lib/component/brush");  //刷选

require("./lib/component/title");  //标题

require("./lib/component/timeline");  //时间轴

require("./lib/component/markPoint");  //标注

require("./lib/component/markLine");  //标线

require("./lib/component/markArea");  //标域

require("./lib/component/legendScroll");

require("./lib/component/legend");  //图例

require("./lib/component/dataZoom");

require("./lib/component/dataZoomInside");

require("./lib/component/dataZoomSlider");

require("./lib/component/visualMap");

require("./lib/component/visualMapContinuous");

require("./lib/component/visualMapPiecewise");

require("zrender/lib/vml/vml");

require("zrender/lib/svg/svg");
```



#### 引入流程

```javascript
//按需引入
// 引入 ECharts 主模块
let echarts = require("echarts/lib/echarts");
// 引入折线图
require("echarts/lib/chart/line");
// 引入提示框和标题组件
require("echarts/lib/component/tooltip");
require("echarts/lib/component/title");


//初始化
//当charts的父元素宽度为百分比时  要在this.$nextTick(()=>{})中来初始化表格
this.$nextTick(()=>{
    let lineChart = echarts.init(this.$refs.linechartf);
    lineChart.showLoading(); //显示等待图
})


//请求数据
this.lineChart.hideLoading(); //隐藏等待图
this.lineOption.xAxis.data = this.cemsLineOption.xdata; //x轴数据
this.lineOption.series[0].data = this.cemsLineOption.ydata; //y轴数据
this.lineChart.setOption(this.lineOption);//应用


//折线图配置
lineOption: {
        title: {
          text: ""  //标题
        },
        tooltip: {
          trigger: "axis",  //触发类型  
          formatter: "{c}" //提示文本
        },
        grid: {  //绘图网格
          left: "3%",
          right: "3%",
          bottom: "20",
          containLabel: true
        },
        xAxis: {
          type: "category",
          boundaryGap: ["2%", "2%"],
          axisLabel: { //x轴
            color: "#333",
            interval: 0, //文本显示，0全显示
            rotate: "45" //文本倾斜度数
          },
          axisLine: {
            show: false
          },
          axisTick: {
            lineStyle: {
              color: "#fff"
            }
          },
          data: [] //x轴数据
        },
        yAxis: {
          type: "value",
          axisLabel: {
            color: "#333"
          },
          axisLine: {
            show: false
          }
        },
        series: [
          {
            data: [], //y轴数据
            type: "line",
            smooth: true,
            areaStyle: {
              // 线性渐变，前四个参数分别是 x0, y0, x2, y2, 范围从 0 - 1，相当于在图形包围盒中的百分比，如果 globalCoord 为 `true`，则该四个值是绝对的像素位置
              color: {
                type: "linear",
                x: 0,
                y: 0,
                x2: 0,
                y2: 1,
                colorStops: [
                  {
                    offset: 0,
                    color: "#81befd" // 0% 处的颜色
                  },
                  {
                    offset: 0.4,
                    color: "#e4f2ff" // 100% 处的颜色
                  },
                  {
                    offset: 1,
                    color: "#fff" // 100% 处的颜色
                  }
                ],
                global: false // 缺省为 false
              }
            },
            itemStyle: {
              normal: {
                color:'#0085ff', //折点颜色
                lineStyle: {
                  color: "#0085ff", //折线颜色
                  width:5,
                }
              }
            },
			symbolSize: 6, //折线点的大小
          }
        ]
      },
```

