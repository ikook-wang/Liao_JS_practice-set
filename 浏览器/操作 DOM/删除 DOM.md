对于一个已有的HTML结构：

```
<!-- HTML结构 -->
<ul id="test-list">
    <li>JavaScript</li>
    <li>Swift</li>
    <li>HTML</li>
    <li>ANSI C</li>
    <li>CSS</li>
    <li>DirectX</li>
</ul>
```

把与Web开发技术不相关的节点删掉：

```
'use strict';

/*  ----- 我的版本。缺点：可以解决本题，但非通用的写法。如果节点位置变化，或者待删除节点并非规律性排序，这种写法则不能够完成删除 --------

var list = document.getElementById('test-list');

for(var i = 0; i < list.children.length; i++) {
     list.removeChild(list.children[i+1]);
}
*/


/*  ------ 网友版本。可以解决以上版本的问题 ----------

var noWebNode =['Swift','ANSI C','DirectX'];
var test_list =document.getElementById("test-list");

for( x of test_list.children){

     if(noWebNode.indexOf(x.innerText)!=-1){
          test_list.removeChild(x)
     }

}
*/

// ------ 我在网友版本上的小优化。新增「Web 开发技术不相关的节点」时亦可直接调用----------


var noWebNode =['JavaScript','HTML','CSS'];
var test_list =document.getElementById("test-list");

for( x of test_list.children){

     if(noWebNode.indexOf(x.innerText)){
          test_list.removeChild(x)
     }

}

// 测试:
;(function () {
    var
        arr, i,
        t = document.getElementById('test-list');
    if (t && t.children && t.children.length === 3) {
        arr = [];
        for (i = 0; i < t.children.length; i ++) {
            arr.push(t.children[i].innerText);
        }
        if (arr.toString() === ['JavaScript', 'HTML', 'CSS'].toString()) {
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