#### 1.
利用 filter，去除 Array 的重复元素：

```
'use strict';

var
    r,
    arr = ['apple', 'strawberry', 'banana', 'pear', 'apple', 'orange', 'orange', 'strawberry'];
    
r = arr.filter(function (element, index, self) {
   /* console.log(self.indexOf(element));
    console.log(index);
    console.log(self.indexOf(element) === index);*/
    return self.indexOf(element) === index;
});
console.log(r.toString());
```
去除重复元素依靠的是indexOf总是返回第一个元素的位置，后续的重复元素位置与indexOf返回的位置不相等，因此被filter滤掉了。


#### 2. 

请尝试用filter()筛选出素数：

```
'use strict';

function get_primes(arr) {
    return arr.filter(function(x){
      
        if(x < 2){
            return false;
        } 
        for (var i = 2; i <= x/2 ; i++){
            if(x%i === 0) {
                return false;
            }
        }
        return true;
    });
}

// 测试:
var
    x,
    r,
    arr = [];
for (x = 1; x < 100; x++) {
    arr.push(x);
}
r = get_primes(arr);
if (r.toString() === [2, 3, 5, 7, 11, 13, 17, 19, 23, 29, 31, 37, 41, 43, 47, 53, 59, 61, 67, 71, 73, 79, 83, 89, 97].toString()) {
    console.log('测试通过!');
} else {
    console.log('测试失败: ' + r.toString());
}
```