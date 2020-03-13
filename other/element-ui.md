### 使用element遇到的问题

#### 1. el-table  表格

1. 动态渲染表格时，要将<el-table-column></el-table-column>标签中有template的单独列出来，否则在template标签内做判断会使其他没有template标签的数据不显示



#### 2. el-tree  树

1. el-tree  在显示隐藏时不会去掉选中状态  通过vif来重新加载el-tree   达到初始化目的
2. el-tree  获取节点信息与回显 要用其中的方法  不能用data属性

