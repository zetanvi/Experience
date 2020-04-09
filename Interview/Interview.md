# 面试常见题

### 输入URL到地址栏后发生了什么？

 ```mermaid
graph TD
A[URL] --> B(DNS解析)
    B --> C[本地hosts文件查询]
    C --> D{host文件是否有该域名}
    D --> |是| E[获得IP]
    D --> |否| F[请求DNS服务器查询]
    F --> E[获得IP]
    E --> G(发起http请求)
    G --> H[通过TCP三次握手获得数据发给客户端]
    H --> I[客户端收到数据生成DOM树 CSS Rule树 两者共同渲染出Render树]
    I --> J(完成渲染生成页面)
 ```