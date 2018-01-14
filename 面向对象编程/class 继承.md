请利用class重新定义Cat，并让它从已有的Animal继承，然后新增一个方法say()，返回字符串'Hello, xxx!'：
```
'use strict';

class Animal {
    constructor(name) {
        this.name = name;
    }
}

class Cat extends Animal {
      
      constructor(name) {
          super(name);
      }
      say() {
          return 'Hello, ' + this.name + '!';
      }

}

// 测试:
var kitty = new Cat('Kitty');
var doraemon = new Cat('哆啦A梦');
if ((new Cat('x') instanceof Animal) && kitty && kitty.name === 'Kitty' && kitty.say && typeof kitty.say === 'function' && kitty.say() === 'Hello, Kitty!' && kitty.say === doraemon.say) {
    console.log('测试通过!');
} else {
    console.log('测试失败!');
}
```