```javascript
let 我 = '...'; //允许
```

```javascript
prompt(title, [default]);//可以指定初始值
```

```javascript
两个非运算 !! 有时候用来将某个值转化为布尔类型
```

```javascript
a ?? b 的结果是：
如果 a 是已定义的，则结果为 a，
如果 a 不是已定义的，则结果为 b。
```

```javascript
  case 2:
  case 3:
    alert( '2,3' );
```

```javascript
空值的 return 和 return undefined 等效：
```

```javascript
function getInfo(userObj){...}, 函数调用: getInfo({name: '', id: '', age: ''})
```

```javascript
function sayHi() {   // (1) 创建
  alert( "Hello" );
}
let func = sayHi;    // (2) 复制
func(); // Hello     // (3) 运行复制的值（正常运行）！
```

```javascript
函数表达式是在代码执行到达时被创建，并且仅从那一刻起可用。
在函数声明被定义之前，它就可以被调用。
sayHi("John"); // Hello, John
function sayHi(name) {
  alert( `Hello, ${name}` );
}
```

```javascript
let sum = (a, b) => {  // 花括号表示开始一个多行函数
  let result = a + b;
  return result; // 如果我们使用了花括号，那么我们需要一个显式的 “return”
};
alert( sum(1, 2) ); // 3
```

```javascript
[1, 2].forEach(alert)
```

```javascript
number — 可以是浮点数，也可以是整数，
bigint — 用于任意长度的整数，
string — 字符串类型，
boolean — 逻辑值：true/false，
null — 具有单个值 null 的类型，表示“空”或“不存在”，
undefined — 具有单个值 undefined 的类型，表示“未分配（未定义）”，
object 和 symbol — 对于复杂的数据结构和唯一标识符，我们目前还没学习这个类型。
```

```javascript
Mocha —— 核心框架：提供了包括通用型测试函数 describe 和 it，以及用于运行测试的主函数。
Chai —— 提供很多断言（assertion）支持的库。它提供了很多不同的断言，现在我们只需要用 assert.equal。
Sinon —— 用于监视函数、模拟内建函数和其他函数的库，我们在后面才会用到它。

<!DOCTYPE html>
<html>
<head>
  <!-- add mocha css, to show results -->
  <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/mocha/3.2.0/mocha.css">
  <!-- add mocha framework code -->
  <script src="https://cdnjs.cloudflare.com/ajax/libs/mocha/3.2.0/mocha.js"></script>
  <script>
    mocha.setup('bdd'); // minimal setup
  </script>
  <!-- add chai -->
  <script src="https://cdnjs.cloudflare.com/ajax/libs/chai/3.5.0/chai.js"></script>
  <script>
    // chai has a lot of stuff, let's make assert global
    let assert = chai.assert;
  </script>
</head>

<body>

  <script>
    function pow(x, n) {
      /* function code is to be written, empty now */
    }
  </script>

  <!-- the script with tests (describe, it...) -->
  <script src="test.js"></script>

  <!-- the element with id="mocha" will contain test results -->
  <div id="mocha"></div>

  <!-- run tests! -->
  <script>
    mocha.run();
  </script>
</body>
</html>
```

```javascript
为了创建“真正的拷贝”（一个克隆），我们可以使用 Object.assign 来做所谓的“浅拷贝”（嵌套的对象通过引用进行拷贝）或者使用“深拷贝”函数，例如 _.cloneDeep(obj)。
```

```javascript
存储在对象属性中的函数被称为“方法”。
方法可以将对象引用为 this
```

```javascript
?. 安全访问
?.() ?.[]用于调用一个可能不存在的函数。
alert( user1?.[key] ); // John
userGuest.admin?.(); // 啥都没发生（没有这样的方法）
```

```javascript
Symbol("id") //里面是一个标志
Symbol.for("id") //全局的

let globalSymbol = Symbol.for("name");
let localSymbol = Symbol("name");
alert( Symbol.keyFor(globalSymbol) ); // name，全局 symbol
alert( Symbol.keyFor(localSyvmbol) ); // undefined，非全局
```

```javascript
如果需对象类型转换的话首先对象会执行
Symbol.toPrimitive
obj[Symbol.toPrimitive] = function(hint) {
  // 这里是将此对象转换为原始值的代码
  // 它必须返回一个原始值
  // hint = "string"、"number" 或 "default" 中的一个
}
如果没有
找tostring方法，没有找valueOf
```

```javascript
Math.floor//向下舍入
Math.ceil//向上舍入
Math.round//向最近的整数舍入
```

```javascript
~n 等于 -(n+1)
```

```javascript
换句话说，arr.at(i)：
如果 i >= 0，则与 arr[i] 完全相同。
对于 i 为负数的情况，它则从数组的尾部向前数。
```

```javascript
for (let i in arr) — 永远不要用这个遍历数组。
for..in 循环会遍历 所有属性，不仅仅是这些数字属性
```

```javascript
对于许多字母，最好使用 str.localeCompare 方法正确地对字母进行排序
```

```javascript
let str = "test";
alert( str.split('') ); // t,e,s,t
```

```javascript
let range = {
  from: 1,
  to: 5
};

// 1. for..of 调用首先会调用这个：
range[Symbol.iterator] = function() {

  // ……它返回迭代器对象（iterator object）：
  // 2. 接下来，for..of 仅与下面的迭代器对象一起工作，要求它提供下一个值
  return {
    current: this.from,
    last: this.to,

    // 3. next() 在 for..of 的每一轮循环迭代中被调用
    next() {
      // 4. 它将会返回 {done:.., value :...} 格式的对象
      if (this.current <= this.last) {
        return { done: false, value: this.current++ };
      } else {
        return { done: true };
      }
    }
  };
};
// 现在它可以运行了！
for (let num of range) {
  alert(num); // 1, 然后是 2, 3, 4, 5
}
```

```javascript
let obj = {
  name: "John",
  age: 30
};
let map = new Map(Object.entries(obj));

let prices = Object.fromEntries([
  ['banana', 1],
  ['orange', 2],
  ['meat', 4]
]);
// 现在 prices = { banana: 1, orange: 2, meat: 4 }

alert(prices.orange); // 2
```

```javascript
WeakMap 和 Map 的第一个不同点就是，WeakMap 的键必须是对象，不能是原始值：
```

```javascript
Object.keys(obj) —— 返回一个包含该对象所有的键的数组。
Object.values(obj) —— 返回一个包含该对象所有的值的数组。
Object.entries(obj) —— 返回一个包含该对象所有 [key, value] 键值对的数组。
```

```javascript
为了告诉 JavaScript 这不是一个代码块，我们可以把整个赋值表达式用括号 (...) 包起来
```

```javascript
let sum = new Function('a', 'b', 'return a + b');
与普通函数的区别在于函数体和参数在一块
```

```javascript
装饰器，这东西和中间件差不多，但有一点区别javascript是将参数作为上下文传递，一般将函数视为一等公民的都能实现
```

```
属性标志和属性描述符
Object.defineProperties(user, {
  name: { value: "John", writable: false },
  surname: { value: "Smith", writable: false },
  // ...
});
```

```javascript
class User {
  ['say' + 'Hi']() {//可以拼出一个方法名
    alert("Hello");
  }
}
new User().sayHi();
```

```javascript
class Rabbit extends Animal
```



