小明为了和女友庆祝情人节，特意制作了网页，并提前预定了法式餐厅。小明打算用JavaScript给女友一个惊喜留言：
```
'use strict';
var today = new Date();
if (today.getMonth() === 2 && today.getDate() === 14) {
    alert('亲爱的，我预定了晚餐，晚上6点在餐厅见！');
}
```

结果女友并未出现。小明非常郁闷，请你帮忙分析他的JavaScript代码有何问题。

**解析：**

由于 JavaScript 设计问题，Data() 对象获取的月份范围为 0~11；也就是 0 表示 1 月，1 表示 2 月... 11 表示 12 月。所以，getMonth() 返回值应该是 1 而不是 2 ，1 表示 2 月份。