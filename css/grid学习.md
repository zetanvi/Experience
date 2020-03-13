## grid学习

###### 本文是基于阮一峰老师的CSS Grid 网格布局教程做的总结，若有相关问题，前往[阮一峰老师教程](https://www.ruanyifeng.com/blog/2019/03/grid-layout-tutorial.html) 学习。

### 总结：

- gird兼容性：支持chrome、Firefox、Safari、Edge、IE10及以上浏览器，IE浏览器要加上前缀**-ms-**

- 使用grid后要尽量避免再使用**浮动**、**定位**、**display设置**等样式

- grid属于二维布局，能够在X轴和Y轴随意调整内容位置

- html基础结构,parent为容器，son就是项目，而grandson则不受grid的影响

  ```html
  <div class="parent">
      <div class="son">
          <div class="grandson"></div>
      </div>
      <div class="son"></div>
  </div>	
  ```



### grid属性

##### 容器属性

1. display属性  声明容器   

   ```css
   .parent{
       display:grid; /*整个容器为块级元素*/
   }
   .parent{
       display:inline-grid; /*整个容器为行内元素*/
   }
   ```

2. 



































































































































##### 项目属性