### 基本算法

#### 1.素数  

在一般领域来说，判断n是否为素数条件是：在2到n的平方根（Math.sqrt()）包括n的平方根之间，如果没有整数可以整除n，则n是素数

```javascript
let arr = [];
  for (let i = 1; i < 100; i++) {
    arr.push(i);
  }
let isPrimes = arr => {
  return arr.filter((num, index, self) => {
    if (num == 2) {
      return true;
    } else if (num == 1) {
      return false;//规定1不是素数
    } else {
      for (let i = 2; i <= Math.sqrt(num); i++) {
        if (num % i == 0) {
          return false;
        }
      }
      return true;
    }
  });
};
console.log(isPrimes(arr))
```



