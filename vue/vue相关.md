### VUE中的插件

vue-cookie   简单的cookie操作库      https://www.cnblogs.com/wangjiachen666/p/9656383.html 

vue-cropper  图片剪切上传处理     https://github.com/xyxiao001/vue-cropper 

vue-amap  地图    https://elemefe.github.io/vue-amap/#/ 

markdown-it-vue  https://github.com/ravenq/markdown-it-vue 在页面中展示markdown文件

G2  https://g2plot.antv.vision/zh/docs/manual/introduction  开箱即用的图表



### 适合VUE的UI组件

移动端：vant(有赞)    cubeui( https://didi.github.io/cube-ui/#/zh-CN )   mandMobile https://didi.github.io/mand-mobile/#/zh-CN/docs/started 

pc：antdvue( https://www.antdv.com/docs/vue/introduce-cn/ ) iviewui  Vuetify (https://vuetifyjs.com/zh-Hans/  新的 很棒)

小程序：colorui(https://www.color-ui.com/)



### VUE样式

1. 在.vue文件中的style标签加上scoped来独立样式作用域
2. 当加上scoped时，每个html标签都会有一个独特的[data-v-xxx]属性
3. 如果在.vue文件中引用了ui组件，想要修改 **组件里面** 的类名样式时要在类名前加/deep/。



### VUEX

1. state:  用来存放数据状态

2. getter：对state中的数据进行操作（过滤，排序，加减等） 为的是不同组件使用同一套数据处理方法时 只需引用this.$store.getter.xxx(函数名)即可，在xx.vue页面中引用可以使用mapGetter

3. mutation:  用来进行提交更改state中的数据，必须是同步函数，否则难以追踪数据的更改，在xx.vue页面通过调用action来调用mutation

4. action:  可以进行异步操作(请求接口/定时器等)  用来触发mutation函数从而改变state中数据的状态

5. module：将不同页面的数据状态管理独立开来，不过getter、action是暴漏在全局store中，可以通过在每个module模块中添加namespaced属性独立各个模块空间

   ```javascript
   import {mapGetter} from 'vuex'
   computed:{
       ...mapGetter(['xxx','yyyy'])
   }
   ```



### VUE中provide/inject    实现深层父子组件传值/函数

```javascript
//父组件
export default{
    data(){
        return{
            
        }
    },
    provide(){
        return{
            test:'123',
            func:this.fatherfn
        }
    },
     methods:{
        fatherfn(){
            console.log('父组件函数fatherfn')
        }
     }
    
}

//子组件
export default{
    data(){
        return{
            
        }
    },
    inject:['test','func'],
    created(){
        this.func()
        console.log(this.test)
    }
}

```



### VUE遍历对象

```javascript
v-for="(val,key,index) in obj" :key="index"

val-该对象    key-键 
取值的话  用val.name可以拿到 obj.name
```





### VUE自适应插件 

##### 1.安装lib-flexible   (无法转换行内样式中的px)

​	 npm i lib-flexible --save 	

##### 2.引入main.js

```javascript
 //放在引入的最下面  防止引入其他css文件影响
 import 'lib-flexible/flexible.js'  
```

##### 3.删除meta标签

将index.html文件头部的meta标签删除   lib-flexible可以自动生成meta

##### 4.安装px2rem-loader

 npm install px2rem-loader  --save   

 可以将你在文件中写的px转换成rem

##### 5.配置px2rem-loader

  修改build/utils.js  

```javascript
 const cssLoader = {
    loader: 'css-loader',
    options: {
      sourceMap: options.sourceMap
    }
  }
  // pxrem转换
  const px2remLoader = {
    loader: 'px2rem-loader',
    options: {
        remUnit: 192, //pcrem单元 浏览器设计图页面宽度1920     
        // remUnit: 75, //移动端单元  ui设计图宽度750   
    },
};

  function generateLoaders (loader, loaderOptions) {
    const loaders = options.usePostCSS
    ? [cssLoader, px2remLoader]
    : [cssLoader ];

    if (loader) {
      loaders.push({
        loader: loader + '-loader',
        options: Object.assign({}, loaderOptions, {
          sourceMap: options.sourceMap
        })
      })
    }
    if (options.extract) {
      return ExtractTextPlugin.extract({
        use: loaders,
        fallback: 'vue-style-loader',
        publicPath:'../../'
      })
    } else {
      return ['vue-style-loader'].concat(loaders)
    }
  }
```

##### 6.修改flexible.js

```javascript
//找到node_modules文件夹中的flexible.js文件
//找到refreshRem函数
function refreshRem(){
      var width = docEl.getBoundingClientRect().width;
      if (width / dpr > 540) {
          width = width * dpr; //pc端不做限制
          //width = 540 * dpr; 手机自适应方案 大于540的设置成540
      }
      var rem = width / 10;
      docEl.style.fontSize = rem + 'px';
      flexible.rem = win.rem = rem;
  }
```

##### 7.px2rem-loader使用

```javascript
//直接写px，编译后会直接转化成rem ---- 除开下面两种情况，其他长度用这个
//在px后面添加/*no*/，不会转化px，会原样输出。 --- 一般border需用这个
//在px后面添加/*px*/,会根据dpr的不同，生成三套代码。---- 一般字体需用这个
```







### VUE项目兼容IE（头疼）

```javascript
//babel是针对不支持es6的浏览器，将文件中的es6语法转换成es5，Babel默认只转换新的JavaScript语法，而不转换新的API

//babel-polyfill，它会”加载整个polyfill库”，针对编译的代码中新的API(如：promise)进行处理，并且在代码中插入一些帮助函数。
//1.安装
npm install babel-polyfill --save-dev
//2.引入
//在main.js中引入或在http配置文件开头引用
import 'babel-polyfill';
//或在bulid/webpack.base.conf.js中，将entry改成下面
entry: {
    app: ['babel-polyfill', './src/main.js']
  },
//重新启动项目生效
      
 
```



### 环境与依赖

development：开发环境  本地

test:             测试环境     测试服

production：生产环境  线上

1. dependencies  下面的依赖包是要发布到生产环境的(线上) 
2. devDependencies  下面的依赖包是要开发环境使用或对代码进行再压缩再编译的的包





### vue中动态渲染图片固定路径

###### assets和static文件夹的区别

assets:里面的文件会经过webpack进行打包

static:里面的文件不会经过webpack处理，会直接复制到打包目录里(dist/static)

若是在vue文件中动态渲染assets中的图片，需要以require的形式将图片当做模块来引入

如：

```vue
<template>
  <div id="Home">
    <div class="itemarr">
      <div class="picitem floatl" v-for="(item,index) in picArr" :key="index">
        <span>{{item.word}}</span>
        <img :src="item.src" alt="">
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "Home",
  data() {
    return {
      picArr: [
        { word: "企业简介", src: require("../../assets/images/introduce.png") },
        { word: "企业文化", src: require("../../assets/images/culture.png") },
        { word: "办公地点", src: require("../../assets/images/location.png") }
      ]
    };
  },
  methods: {},
  created() {},
  mounted() {},
  computed: {}
};
</script>

<style scoped lang="scss">
</style>

```



### vue小技巧

1.   父组件监听子组件生命周期

```javascript
// 基本方法
//父组件引用
<child @created="fn" />
//子组件
created(){
    this.$emit('created',data)
}
//简洁用法  子组件不用处理  父组件引入时直接通过@hook监听
<child @hook:created="fn" />
<child @hook:mounted="fn" />
```

2. watch监听 

   ```javascript
   //基础用法
   watch:{
       mn(val,oVal){
           //something
       }
   }
   //监听对象中键值变化或数组中的对象变化   深度监听
   watch:{
       mn:{
           handler(val,oVal){
               //something
           },
           deep:true  //这样对象中一个属性值变化时也能监听到
       }
   }
   //监听立即执行
   watch:{
       mn:{
           handler(val,oVal){
               //something
           },
           immediate:true
       }
   }
   ```

3. 当使用路由参数query或params时，参数变化但是path未变化，组件不会实时更新

   ###### 原因：在vue页面获取参数query、params是在created或mounted中，这样路由只有参数发生变化而path没变化（不能触发created和mounted），所以组件不能更新

   ```javascript
   //解决方案  用watch来监听路由中的参数变化(拿到参数)
   watch:{
       '$route'(to,from){
           //get to.query || get to.params
       }
   }
   ```

4. 在离开vue页面时要记得清除该页面的定时器和其他监听事件（resize等）

5. 可以自定义路径别名

   ```javascript
   //  build-->webpack.base.conf.js
   resolve:{
       extensions:['.js','.vue','.json'],
       alias:{
           'vue$':'vue/dist/vue.esm.js',
            '@':resolve('src'),//在其他文件中@就表示从src这个目录下开始
            'assets':resolve('src/assets')
       }
   }
   ```

6. 动态修改dom样式

   ###### 通常vue文件中的样式都会加上scoped来表示独一性，这样就不能动态修改dom样式，解决方法是将需要动态修改样式的dom的类名写在新的不加scoped的style标签中

7. 若只是用来展示的列表list可以通过Object.freeze来冻结对象，这样vue就不会对这个list进行数据劫持，从而优化组件初始化时间。

   Object.freeze() 返回一个只读的浅拷贝对象，即无法更改对象的原有的键和值

8. 内容分发slot  插槽

   ###### slot分三种：匿名插槽/默认插槽、具名插槽/命名插槽、作用域插槽

   ###### 父级模板里的所有内容都是在父级作用域中编译的；子模板里的所有内容都是在子作用域中编译的。

   ```javascript
   //-------默认插槽  子组件中只能有一个
   //父组件
   <child>
   	<div>something</div>    
   </child>
   //子组件 
   <div id="child">
       //用solt来接受父组件传来的html
      <slot></slot>
   </div>
   
   //---------命名插槽  子组件能存在多个
   //父组件
   <child>
   	<div slot="xx"></div>    
   </child>
   //子组件
   <div id="child">
       <slot name="xx"></slot>
   </div>
   
   //--------作用域插槽  
   //父组件
   <child>
       //默认插槽 default标识默认  slotProps可以拿到子组件中slot标签传的值
   	<template v-slot:default="slotProps">
   		something is {{slotProps.xx}}
       </template>   
   </child>
   //子组件
   <div id="child">
   	<slot :xx="yy"></slot>
   </div>
   ```


9. 项目中开发和生产接口设置

   ```javascript
   //在由vue-cli创建的项目中，对接口进行设置，以便自动处理开发和生产环境
   //1.在config/dev.env.js中添加如下代码
       VUE_BASE_URL = '"开发环境公共接口部分"'
   //2.在config/prod.env.js中添加如下代码
   	VUE_BASE_URL = '"生产环境公共接口部分"'
   //3.在接口管理文件中（base.js）
       base_url = process.env.VUE_APP_BASE_API
   //process.env.VUE_APP_BASE_API会根据当前环境自动取值，这样在打包时就不用再去修改接口了。
   ```

   



### 生成Vnode

- 在vue中可以使用**this.$createElement**来生成vnode对象

  ```javascript
  const h = this.$createElement
  h(a,b,c)
  // a-------元素标签  'div'、'span'
  // b-------元素配置  事件、类名、属性等，没有填null  见https://cn.vuejs.org/v2/guide/render-function.html  中的深入数据对象
  // c-------子元素或自身标签内容，一般为数组,不需要可不填   [h('span',null),'miniyellow']此时miniyellow就为自身标签中的内容
  ```

  

