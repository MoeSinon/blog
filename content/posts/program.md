多参数

与golang细微差别实在Rest 参数可以通过使用三个点 `...` 并在后面跟着包含剩余参数的数组名称，来将它们包含在函数定义中。这些点的字面意思是“将剩余参数收集到一个数组中”。

有一个名为 `arguments` 的特殊的类数组对象，该对象按参数索引包含所有参数。如果我们在箭头函数中访问 `arguments`，访问到的 `arguments` 并不属于箭头函数，而是属于箭头函数外部的“普通”函数。

```javascript
function f() {
  let showArg = () => alert(arguments[0]);
  showArg();
}

f(1); // 1
```

```javascript
let arr = [3, 5, 1];

alert( Math.max(...arr) ); // 5（spread 语法把数组转换为参数列表）

let arr1 = [1, -2, 3, 4];
let arr2 = [8, 3, -8, 1];

alert( Math.max(...arr1, ...arr2) ); // 8

let arr1 = [1, -2, 3, 4];
let arr2 = [8, 3, -8, 1];

alert( Math.max(1, ...arr1, 2, ...arr2, 25) ); // 25

let obj = { a: 1, b: 2, c: 3 };

let objCopy = { ...obj }; // 将对象 spread 到参数列表中
                          // 然后将结果返回到一个新对象
let arr = [1, 2, 3];

let arrCopy = [...arr]; // 将数组 spread 到参数列表中
                        // 然后将结果放到一个新数组
```

闭包 (一个记住其外部变量并可以访问这些变量的函数)

词法环境(我们可以使用它来隔离一段代码，该段代码执行自己的任务，并使用仅属于自己的变量)

所有函数都有名为 `[[Environment]]` 的隐藏属性，该属性保存了对创建该函数的词法环境的引用。（以下在嵌套函数中适用）当函数中的代码查找变量时，它首先搜索自己的词法环境（为空，因为那里没有局部变量），然后是外部的词法环境，并且在哪里找到就在哪里修改。

```javascript
function f() {
  let value = Math.random();

  return function() { alert(value); };
}

// 数组中的 3 个函数，每个都与来自一个对应的 f() 的词法环境
let arr = [f(), f(), f()];
```

eg:

```javascript
let phrase = "Hello";
if (true) {
  let user = "John";

  function sayHi() {
    alert(`${phrase}, ${user}`);
  }
}
sayHi();//error
去掉if再看看
```



