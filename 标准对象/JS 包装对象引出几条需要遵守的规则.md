- 不要使用 new Number()、new Boolean()、new String() 创建包装对象；

- 用 parseInt() 或 parseFloat() 来转换任意类型到 number；

- 用 String() 来转换任意类型 string，或者直接调用某个对象的 toString() 方法；

- 通常不必把任意类型转换为 boolean 再判断，因为可以直接写 if (myVar) {...}；

- typeof 操作符可以判断出 number、boolean、string、function 和 undefined；

- 判断 Array 要使用 Array.isArray(arr)；

- 判断 null 请使用 myVar === null；

- 判断某个全局变量是否存在用 `typeof window.myVar === 'undefined'`；

- 函数内部判断某个变量是否存在用 `typeof myVar === 'undefined'`。


任何对象都有 toString() 方法吗？null 和 undefined 就没有！确实如此，这两个特殊值要除外，虽然 null 还伪装成了 object 类型。


number 对象调用 toString() 报 SyntaxError：见文章「[Number 调用 toString() 方法产生的问题](http://www.zuojj.com/archives/888.html)」


