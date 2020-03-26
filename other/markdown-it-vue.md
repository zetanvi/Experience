## markdown-it-vue的使用（https://github.com/ravenq/markdown-it-vue）

#### 1.下载

- `npm install text-loader --save`

- `npm install markdown-it-vue --save`

#### 2.使用

* 配置

  > build文件下的webpack.base.conf.js文件中加上解析markdown文件loader
  >
  > ```javascript
  > // markdown
  > {
  >    test: /\.md$/,
  >    loader: 'text-loader',
  > },
  > ```
  >
  > 

* 页面引入和使用

  > ```vue
  > <template>
  >   <div id="home">
  >     <markdown-it-vue class="md-body" :content="content" />
  >   </div>
  > </template>
  > 
  > <script>
  > import MarkdownItVue from "markdown-it-vue";
  > import "markdown-it-vue/dist/markdown-it-vue.css";
  > import markdown from "../../static/js相关.md";
  > export default {
  >   name: "Home",
  >   data() {
  >     return {
  >       content:markdown
  >     };
  >   },
  >   components:{
  >     MarkdownItVue
  >   },
  >   methods: {
  >   },
  >   computed: {},
  >   created() {}
  > };
  > </script>
  > ```
  >
  > 

#### 3.注意事项

* 因为要引入css样式文件，所以要使用**autoprefixer**来给样式加前缀兼容各种浏览器，

  通过**postcss-cli**安装 `npm install postcss-cli autoprefixer`，

  暂时未使用`npx postcss *.css --use autoprefixer -d build/`(指的是使用autoprefixer处理某个css文件)

* 要是markdown文件显示文档只需要在头部单独行加上**[TOC]**即可