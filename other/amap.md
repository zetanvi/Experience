## 高德地图的使用（版本2.0）

> 在具体业务中，vue-amap已经无法满足复杂的需求操作，因此引入高德地图，使用它提供的api来完成。

#### 使用

- 使用 JSAPI Loader 来加载

  > 安装：npm i @amap/amap-jsapi-loader --save-dev
  >
  > 引入：在使用地图的页面或地图封装组件中：
  >
  > ```javascript
  > import AMapLoader from '@amap/amap-jsapi-loader';
  > 
  > monted(){
  >     AMapLoader.load({
  >     "key": "您申请的key值",   // 申请好的Web端开发者Key，首次调用 load 时必填
  >     "version": "2.0",   // 指定要加载的 JSAPI 的版本，缺省时默认为 1.4.15
  >     "plugins": []  //插件列表
  >     }).then((AMap)=>{
  >         //地图的一些方法或监听点击事件或使用插件功能
  >         map = new AMap.Map('container'，{
  > 			//地图属性
  >         });
  >         
  >     }).catch(e => {
  >         console.log(e);
  >     })
  > }
  > destroyed() {
  >     //离开时销毁地图
  >     this.map.destroy();
  >   }
  > ```
  >
  > 

### 常用方法和插件

- 点击事件：`this.map.on("click", this.mapClick); `
- 地理编程：

```javascript
this.map.plugin("AMap.Geocoder", () => {
    this.geocoder = new AMap.Geocoder();
});
//经纬度转为地址
this.geocoder.getAddress(marker, (status, result) => {
    if (status === "complete" && result.info === "OK") {
        console.log(result, "result");
    }
});
//地址转为经纬度
this.geocoder.getLocation(this.searchInfo, (status, result) => {
    if (status === "complete" && result.info === "OK") {
        // 在这里拿到经纬度 然后赋值给this.markers和this.center
        let { lng, lat } = result.geocodes[0].location;
        this.map.setZoomAndCenter(10, [lng, lat]);
    }
});
```

- 点聚合

```javascript
this.map.plugin(["AMap.MarkerClusterer"], () => {
    this.cluster = new AMap.MarkerClusterer(
        this.map, // 地图实例
        this.markers, // 海量点数据，数据中需包含经纬度信息字段 lnglat
        {
            gridSize: 60, // 聚合网格像素大小
            maxZoom: 16
        }
    );
    this.cluster.on("click", this.clusterClick);
});
```

