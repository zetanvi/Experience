### vue-amap  地图    https://elemefe.github.io/vue-amap/#/ 

#### vue-amap使用准备

```javascript
// 1. npm 安装
npm install vue-amap --save
//2. 在main.js中引入
import Vue from 'vue';
import VueAMap from 'vue-amap';
Vue.use(VueAMap)
VueAMap.initAMapApiLoader({
    key:'your amap key',  //可以在高德开放平台申请 https://lbs.amap.com/  
    //保时安物联网   e9812a231466a311c4339cd719cdf413
    //保时安后台map  49dd214f6065041835c0f908667444d0
    plugin: ['AMap.Autocomplete', 'AMap.PlaceSearch', 'AMap.Scale', 'AMap.OverView', 'AMap.ToolBar', 'AMap.MapType', 'AMap.PolyEditor', 'AMap.CircleEditor','Geocoder'], //引入高德插件
  v: '1.4.4' // 默认高德 sdk 版本为 1.4.4
})
// 3. vue页面使用
import { AMapManager } from "vue-amap"; //引入
let amapManager = new AMapManager(); //实例化地图
data(){
    return{
        amapManager,//地图实例
        events: { //地图事件对象
            init: o => {
              // console.log(o.getCenter());
              // console.log(this.$refs.map.$$getInstance());
              o.getCity(result => {
                // console.log(result);
              });
            },
            moveend: () => {},
            zoomchange: () => {},
            click: e => {  
              // console.log(e, "e", e.lnglat.O, e.lnglat.P);
              //点击地图添加点标志
              this.markers.length = 0;
              let marker = {
                position: [e.lnglat.O, e.lnglat.P]
              };
              this.markers.push(marker);
              let { lng, lat } = e.lnglat; //经纬度坐标
              var geocoder = new AMap.Geocoder({
                radius: 1000,
                extensions: "all"
              });
                //经纬度转换实际地址
              geocoder.getAddress([lng, lat], (status, result) => {
                if (status === "complete" && result.info === "OK") {
                  if (result && result.regeocode) {
                    let realPosition = result.regeocode.formattedAddress;
                    this.$nextTick();
                  }
                }
              });
            }
      },
        markers: [],//点标志集合
        center: [113.55084, 34.7885],//地图初始中心点 可动态修改
    }
}
```

```html
<!-- 
template引入
-->
<div id="mapc">
   <el-amap :amap-manager="amapManager" :events="events" :vid="'amap-vue'" class="amap-box" ref="map" :center="center">
    <el-amap-marker :key="index" :position="item.position" v-for="(item,index) in markers">
      </el-amap-marker>
     </el-amap>
</div>
```

```scss
//css样式   一定要给一个有高度的容器
 #mapc {
        margin: 0 auto;
        height: 300px;
   }
```

