对于一个已有的HTML结构：
```
<!-- HTML 结构 -->
<ol id="test-list">
    <li class="lang">Scheme</li>
    <li class="lang">JavaScript</li>
    <li class="lang">Python</li>
    <li class="lang">Ruby</li>
    <li class="lang">Haskell</li>
</ol>
```

按字符串顺序重新排序DOM节点：

```
'use strict';

// sort list:


/*  -------- 错误的版本。只是将 DOM 节点的内容进行了排序之后重新赋值给原 DOM 节点，并没有重新排序 DOM 节点。虽然可以通过排序测试，但不符合题意------------

var list = document.getElementById('test-list');

var arr = [];

for(var i = 0; i < list.children.length; i++) {
     
   arr [i] = list.children[i].innerText;    

}

arr.sort();
console.log(arr);

for(var i = 0; i < arr.length; i++) {
     
   list.children[i].innerText = arr[i];

}
*/


// -------- 改进后的版本。将 DOM 节点保存到数组中，然后利用数组的 sort() 方法「这里重新定义了 sort() 的排序方式，根据 DOM 节点的内容进行排序」 ------------

var list = document.getElementById('test-list');

var arr = [];

for(var i = 0; i < list.children.length; i++) {
     
   arr [i] = list.children[i];    

}

console.log(arr);

arr.sort(function(x,y) {

    return x.innerText > y.innerText;

});


for(var i = 0; i < arr.length; i++) {
     
   list.appendChild(arr[i]);  // 如果插入的 DOM 节点已经存在于当前的文档树，这个节点首先会从原先的位置删除，再插入到新的位置。

}



/* -------- 网友的版本，这里利用了 Array.from() 方法将节点复制给数组 arr，简化了利用 for 循环来复制的方法。 ------------

var list = document.getElementById("test-list");
var arr = Array.from(list.children);

console.log(arr);

arr.sort(function(x, y) {
   return x.innerText > y.innerText;
});

for(let i = 0; i < arr.length; i++) {
    list.appendChild(arr[i]);
}*/

// 测试:
;(function () {
    var
        arr, i,
        t = document.getElementById('test-list');
    if (t && t.children && t.children.length === 5) {
        arr = [];
        for (i=0; i<t.children.length; i++) {
            arr.push(t.children[i].innerText);
        }
        if (arr.toString() === ['Haskell', 'JavaScript', 'Python', 'Ruby', 'Scheme'].toString()) {
            console.log('测试通过!');
        }
        else {
            console.log('测试失败: ' + arr.toString());
        }
    }
    else {
        console.log('测试失败!');
    }
})();
```