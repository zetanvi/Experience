### 优雅算法

#### 1.去重

```javascript
//原理：indexOf()只返回数组中第一个该元素的index，后续相同元素的index不相等，因此被filter过滤掉
let arr = [1,1,12,2,2,3,3,3,444,4,5,6,77,7,7,78,8,8,]
let isNotSame = (arr)=>{
    return arr.filter((num,index,self)=>{
        return self.indexOf(num)===index
    })
}
console.log(isNotSame(arr))
```

#### 2.判断奇偶数

```javascript
//普通判断方法  num%2
//高逼格方法  使用&符号  num&1  结果为0时num是偶数，结果为1时num是奇数
```

