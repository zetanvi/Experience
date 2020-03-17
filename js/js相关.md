### 验证

邮箱验证

```javascript
function isEmail (s) {
  return 
    /^([a-zA-Z0-9_-])+@([a-zA-Z0-9_-])+((.[a-zA-Z0-9_-]{2,3}){1,2})$/.test(s)
}
```

手机号验证

```javascript
function isMobile (s) {
  return /^1[0-9]{10}$/.test(s)
}
```

电话号码验证

```javascript
function isPhone (s) {
  return /^([0-9]{3,4}-)?[0-9]{7,8}$/.test(s)
}
```

URL地址验证

```javascript
function isURL (s) {
  return /^http[s]?:\/\/.*/.test(s)
}
```





### 节流与防抖

**相同点：在不影响用户体验的前提下，将频繁的函数触发进行次数缩减，以避免性能消耗和页面卡顿,都可适用于 mousemove、scroll、resize、input 等事件**

**不同点：防抖是将多次的触发变成只执行最后一次事件的触发，节流是在一定时间内只触发一次函数**

**适用：*防抖*  就像法师放技能前的吟唱，需要一段时间才能释放，若是途中打断则需要在此吟唱(等待一段时间)，最后只触发一次；search搜索联想/input输入发出请求时，window触发resize的时候，不断调整浏览器最后只触发一次。  *节流*  鼠标不断拖动触发，mousedown(每几秒内触发一次)，监听滚动事件scroll，加载更多**

​		

```javascript
//防抖
debounce(fn,wait){
    //fn->要执行的函数   wait->规定时间
    let timer = null
    return ()=>{
        if(timer!==null) clearTimeout(timer)
        timer = setTimeout(fn,wait)
    }
}
yourfn(){
    console.log(Math.random())
}
//滚动触发
window.addEventListener('scroll',debounce(yourfn,1000))  
//视窗大小改变触发
window.addEventListener('resize',debounce(yourfn,1000))  
```



```javascript
//节流
throttle(fn,delay){
    let prev = Date.now()
    return ()=>{
        let now = Date.now()
        if(now-prev>delay){
            fn()
            prev = Date.now()
        }
    }
}

yourfn(){
    console.log(Math.random())
}
//滚动触发
window.addEventListener('scroll',throttle(yourfn,1000))  
//视窗大小改变触发
window.addEventListener('resize',throttle(yourfn,1000)) 
```



### Date自定义格式化日期

```javascript
Date.prototype.toLocaleString = function(){
    let addZero = (num)=>{
        if(num<10) return '0'+num
        return num
    }
    return this.getFullYear()+'/'+addZero(this.getMonth()+1)+'/'+addZero(this.getDate())+' '+addZero(this.getHours())+':'+addZero(this.getMinutes())+':'+addZero(this.getSeconds())
}
let time = new Date(Date.now()).toLocaleString
console.log(`现在时间时${time}`)
```









### 对象键为特殊符号时的取值方法

```javascript
let obj = {%:1,小黄:2}
console.log(obj['%'],obj['小黄'])
```



### 上传方式

```javascript

```





### 下载Excel方式

```javascript
 //1.后端完全处理  直接下载  使用创建表单方式 input相当于参数
 	  let formElement = document.createElement("form");  //创建表单
      formElement.style.display = "display:none;";  //隐藏表单
      formElement.method = "get";   //请求方式
      formElement.action = "http://localhost:8081/bsa-admin/hgas/exportExcel";  //请求地址
      formElement.target = "callBackTarget";  //页面打开方式
      let inputElement = document.createElement("input");  //创建input
      inputElement.type = "hidden"; 
      inputElement.name = "engineId";  //相当于参数名
      inputElement.value = this.hostDatax.engine_id;  //参数的具体值
      formElement.appendChild(inputElement);  //将input放入form中
      if (this.starTime) {
        let inputElement3 = document.createElement("input");
        inputElement3.type = "hidden";
        inputElement3.name = "starttime";
        inputElement3.value = this.starTime;
        let inputElement4 = document.createElement("input");
        inputElement4.type = "hidden";
        inputElement4.name = "endtime";
        inputElement4.value = this.endTime;
        formElement.appendChild(inputElement3);
        formElement.appendChild(inputElement4);
      }
      document.body.appendChild(formElement);  //将form放到body中
      formElement.submit();  //提交表单
      document.body.removeChild(formElement);  //移除表单元素


//2.前端处理返回的文件流 常用get请求 
axios.get(`${base.baseurl}/hair/exportExcel?engineId=${params.engineId}&time=${params.time}&yinzi=${params.yinzi}&starttime=${params.startime}&endtime=${params.endtime}`,{responseType: 'blob'})
//请求加上{responseType: 'blob'}
 this.$api.atmosphere.airHisDataExcel(obj).then(res => {
          const blob = new Blob([res.data], {
            type: "application/vnd.openxmlformats-officedocument.spreadsheetml.sheet;charset=utf-8"
          }); // application/vnd.openxmlformats-officedocument.spreadsheetml.sheet这里表示xlsx类型
          const downloadElement = document.createElement("a");
          const href = window.URL.createObjectURL(blob); // 创建下载的链接
          downloadElement.href = href;
          downloadElement.download = "大气环境历史数据.xlsx"; // 下载后文件名
          document.body.appendChild(downloadElement);
          downloadElement.click(); // 点击下载
          document.body.removeChild(downloadElement); // 下载完成移除元素
          window.URL.revokeObjectURL(href); // 释放掉blob对象
      });
```



### axios链式请求

```javascript
axios.post(`${base.baseurl}/gas/getOneEngine`, qs.stringify(params)).then(res=>{
    console.log(res)
    //若下一个接口需要该接口获取的数据则需要return
    return res
}).then(res=>{
    axios.get(`/lalal/${res.data}`).then(res=>{
        console.log('可以一直下去')
    })
})
```



### String 方法

```javascript
 //替换   reg是个正则，string字符串 ，不改变str 返回值是替换后的str
	str.replace(reg,string)
 //截取   star是开始截取的底标 end是结束的底标(截取的值不包含end)，不传表示到结尾 返回值是截取的值
	str.substring(star,end)
```



### 浏览器密码自动填充解决

```javascript
//问题：当页面出现有input[type="password"]时，不管input是不是隐藏状态(display:none;)，浏览器会推荐在该输入框中填充密码，在该页面第一个input[type="text"]中填充用户名。
//解决方案：在页面最前端加上两个隐藏的input，input[type="text"]input[type="password"]
```



