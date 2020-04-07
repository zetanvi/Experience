# mavonEditor的使用(https://github.com/hinesboy/mavonEditor)

> ​	mavonEditor是一个在vue项目中用来展示和编辑markdown文件的插件

### 引入

```
//安装
npm install mavon-editor --save
```

### 使用

```javascript
//main.js全局引用
    import Vue from 'vue'
    import mavonEditor from 'mavon-editor'
    import 'mavon-editor/dist/css/index.css'
// use
	Vue.use(mavonEditor)
```

```html
<!--容器-->
<div id="main">
    <mavon-editor v-model="value"/>
</div>
```

### 图片上传

```vue
<template>
    <mavon-editor ref=md @imgAdd="$imgAdd" @imgDel="$imgDel"></mavon-editor>
</template>

<script>
export default {
  methods: {
      // 绑定@imgAdd event
        $imgAdd(pos, $file){
            // 第一步.将图片上传到服务器.
           var formdata = new FormData();
            //这里的'image'就是后端接口中的参数名
           formdata.append('image', $file);
           axios({
               url: 'server url',
               method: 'post',
               data: formdata,
               headers: { 'Content-Type': 'multipart/form-data' },
           }).then((url) => {
               // 第二步.将返回的url替换到文本原位置![...](0) -> ![...](url)
               /**
               * $vm 指为mavonEditor实例，可以通过如下两种方式获取
               * 1. 通过引入对象获取: `import {mavonEditor} from ...` 等方式引入后，`$vm`为`mavonEditor`
               * 2. 通过$refs获取: html声明ref : `<mavon-editor ref=md ></mavon-editor>，`$vm`为 `this.$refs.md`
               */
               $vm.$img2Url(pos, url);
           })
        }，
      //删除图片时要把服务器上的数据删掉
      $imgDel(pos) {
      this.$api.pictureNet.deletePic({ url: pos[0] }).then(res => {
        if (res.data.code === 0) {
          this.$Message({
            message: res.data.msg,
            type: "success",
            center: true
          });
        } else {
          this.$Message({
            message: res.data.msg,
            type: "error",
            center: true
          });
        }
      });
    },
  }
};
</script>
```

### TIPS

+ ###### 可以使用**markdown-body**这个类名来修改样式

+ ###### 可以用字符串形式来保存文件内容，这样再次回显或预览时就可以直接赋值给value来显示