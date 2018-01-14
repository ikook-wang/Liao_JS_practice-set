### map()

由于map()方法定义在JavaScript的Array中，我们调用Array的map()方法，传入我们自己的函数，就得到了一个新的Array作为结果：

```
'use strict';

function pow(x) {
    return x * x;
}

var arr = [1, 2, 3, 4, 5, 6, 7, 8, 9];
var results = arr.map(pow); // [1, 4, 9, 16, 25, 36, 49, 64, 81]

console.log(results);
```


### reduce()

利用reduce()求积：

```
'use strict';

function product(arr) {
    return  arr.reduce((x,y) => x*y);
}

// 测试:
if (product([1, 2, 3, 4]) === 24 && product([0, 1, 2]) === 0 && product([99, 88, 77, 66]) === 44274384) {
    console.log('测试通过!');
}
else {
    console.log('测试失败!');
}
```

### 综合：
#### 1.
重点看：<br>
不要使用JavaScript内置的parseInt()函数，利用map和reduce操作实现一个string2int()函数：

解法1：不够简洁
```
'use strict';

function string2int(s) {
    var arr = [];
    
    // 使用循环将字符串 s 的元素拆解并赋值给数组 arr。
    for (var i = 0; i < s.length; i++) {
        arr[i] = s[i];
    }

    console.log(arr);
    // 重点在于使用 map() 方法将数组 arr 元素转换为 Number 类型。
    return arr.map(x=>+x).reduce((x,y) => x * 10 + y);
}

// 测试:
if (string2int('0') === 0 && string2int('12345') === 12345 && string2int('12300') === 12300) {
    if (string2int.toString().indexOf('parseInt') !== -1) {
        console.log('请勿使用parseInt()!');
    } else if (string2int.toString().indexOf('Number') !== -1) {
        console.log('请勿使用Number()!');
    } else {
        console.log('测试通过!');
    }
}
else {
    console.log('测试失败!');
}
```
解法2：最优解。
```
'use strict';

function string2int(s) {
   return s.split('').map( x => +x ).reduce( (x, y) => x * 10 + y );
}

// 测试:
if (string2int('0') === 0 && string2int('12345') === 12345 && string2int('12300') === 12300) {
    if (string2int.toString().indexOf('parseInt') !== -1) {
        console.log('请勿使用parseInt()!');
    } else if (string2int.toString().indexOf('Number') !== -1) {
        console.log('请勿使用Number()!');
    } else {
        console.log('测试通过!');
    }
}
else {
    console.log('测试失败!');
}
```
思考 map() 将数组元素转换为 Number 类型的过程：
```
var arr = '12345';

console.log(arr.split('').map( x => x ));
//console.log(arr.split('').map( x => +x ));
var arr2 = ["1","2","3"];

var arr3 = [];

for(var key in arr2){
   arr3[key] = +arr2[key]; // 一元加法(+): 将操作数转化为数字。
}
console.log(arr3);
```

#### 2.
请把用户输入的不规范的英文名字，变为首字母大写，其他小写的规范名字。输入：['adam', 'LISA', 'barT']，输出：['Adam', 'Lisa', 'Bart']。

```
'use strict';

function normalize(arr) {
    return arr.map(x => x.substring(0,1).toUpperCase() + x.substring(1).toLowerCase());
}

// 测试:
if (normalize(['adam', 'LISA', 'barT']).toString() === ['Adam', 'Lisa', 'Bart'].toString()) {
    console.log('测试通过!');
}
else {
    console.log('测试失败!');
}
```

#### 3.

小明希望利用map()把字符串变成整数，他写的代码很简洁：
```
'use strict';

var arr = ['1', '2', '3'];
var r;
r = arr.map(parseInt);

console.log(r);
```
结果竟然是`1, NaN, NaN`，小明百思不得其解，请帮他找到原因并修正代码。

**正解：**
```
'use strict';

var arr = ['1', '2', '3'];
var r;
r = arr.map(Number);

console.log(r);
```
**解析：**

由于 map() 接收的回调函数可以有 3 个参数：`callback(currentValue, index, array)`，通常我们仅需要第一个参数，而忽略了传入的后面两个参数。不幸的是，`parseInt(string, radix)` 没有忽略第二个参数，导致实际执行的函数分别是：

- parseInt('0', 0); // 0, 按十进制转换
- parseInt('1', 1); // NaN, 没有一进制
- parseInt('2', 2); // NaN, 按二进制转换不允许出现2

可以改为`r = arr.map(Number);`，因为`Number(value)`函数仅接收一个参数。