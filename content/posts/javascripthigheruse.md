JavaScript对象属性访问

```javascript
writable //如果为 true，则值可以被修改，否则它是只可读的。
enumerable //如果为 true，则会被在循环中列出，否则不会被列出。
configurable //如果为 true，则此属性可以被删除，以上特性也可以被修改，否则不可以。
```

```javascript
Object.getOwnPropertyDescriptor(obj, propertyName);
//一次获取所有属性描述符,一起可以用作克隆对象的“标志感知”方式
Object.defineProperties({},Object.getOwnPropertyDescriptors(obj));
Object.defineProperty(obj, propertyName, descriptor)
//允许一次定义多个属性
Object.defineProperties(obj, {
  prop1: descriptor1,
  prop2: descriptor2
  // ...
});

let user = {};
Object.defineProperty(user, "name", {
  value: "John"
});
let descriptor = Object.getOwnPropertyDescriptor(user, 'name');
alert( JSON.stringify(descriptor, null, 2 ) );
Object.defineProperty(user, "name", {
  writable: false
});
user.name = "Pete"; //通过更改 writable 标志来把 user.name 设置为只读（user.name 不能被重新赋值
/*
{
  "value": "John",
  "writable": false,
  "enumerable": false,
  "configurable": false
}
 */

```

原型

对象有一个特殊的隐藏属性 `[[Prototype]]`只能有一个 `[[Prototype]]`。一个对象不能从其他两个对象获得继承。

__proto__

```javascript
let animal = {
  eats: true
};
let rabbit = {
  jumps: true
};
rabbit.__proto__ = animal; // 设置 rabbit.[[Prototype]] = animal
// 现在这两个属性我们都能在 rabbit 中找到：
alert( rabbit.eats ); // true (**)
alert( rabbit.jumps ); // true
delete rabbit.jumps; //删除对象属性
```

内建方法 [obj.hasOwnProperty(key)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/hasOwnProperty)：如果 `obj` 具有自己的（非继承的）名为 `key` 的属性，则返回 `true`。

1. 类总是使用 `use strict`。 在类构造中的所有代码都将自动进入严格模式。

错误处理

try...catch

如果在“计划的（scheduled）”代码中发生异常，例如在 `setTimeout` 中，则 `try...catch` 不会捕获到异常

除非：

```javascript
setTimeout(function() {
  try {
    noSuchVariable; // try...catch 处理 error 了！
  } catch {
    alert( "error is caught here!" );
  }
}, 1000);
```

```javascript
try {
   ... 尝试执行的代码 ...
} catch (err) {
   ... 处理 error ...
} finally {
   ... 总是会执行的代码 ...
}
```

回调

