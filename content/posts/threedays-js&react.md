JavaScripts基础赏析 基于ES6，现代javascript基础以及函数式编程

首先你必须得知道

什么是值，**primitive** and **object**

**primitive**：null，undefined和8种类型，另外要注意"number"类型其实是int

**object**：数组，是的数组也是对象，在golang中你也可以将数组视为对象

```javascript
typeof undefined;           // "undefined"
typeof null;                // "object" -- oops, bug!
null === undefined;     // false"这是一个和number"
NaN === NaN;            // false
0 === -0;               // true
```

以上当出现对象之间进行比较采用引用比较，就算是相同的结构体也无法三等，或许这和golang有些相同

除此之外还有闭包，这也是和golang相似的地方，你会发现闭包是存在状态的，在每次调用该闭包时都会更新。闭的是变量，而不仅仅是值的快照，或者你可以将其视为回调的一种，只不过你在手动操作而已

```javascript
function counter(step = 1) {
    var count = 0;
    return function increaseCount(){
        count = count + step;
        return count;
    };
}

var incBy1 = counter(1);
var incBy3 = counter(3);

incBy1();       // 1
incBy1();       // 2

incBy3();       // 3
incBy3();       // 6
incBy3();       // 9
```

```go
func counter(a int) func() int {
	var b int
	return func() int {
		b = b + a
		return b
	}
}

func main() {
	var c = counter(1)
	log.Println(c())
	log.Println(c())
	log.Println(c())
}
```

```javascript

var const let
其中var的使用方式和golang是完全一致的，但效果上不一样
var在声明变量之后执行时会将声明优先执行，注意不是赋值
var声明局部变量之作用在函数域当然也包括全局域，对于语法块来说无法声明局部变量，什么是语法块，比如for循环语句

let
至少在ES6中无法在同一个语句块中重复声明，注意不是赋值
并且与const相同无法优先声明

const
在let基础又加了一层限制，无法重新分配即重新赋值
但我们知道一切皆对象，能使用const声明的变量不止8种还有
struct，array我们可以改变其中的成员变量

全局变量
globalThis
// 将当前用户信息全局化，以允许所有脚本访问它
globalThis.currentUser = {
  name: "John"
};

函数
函数即对象，意味着你可以使用对象的方发来操作函数
function sayHi() {
  alert("Hi");
}
alert(sayHi.name); // sayHi，你可以获取函数的名称，即使函数为匿名函数
function f1(a) {}
function f2(a, b) {}
function many(a, b, ...more) {}
alert(f1.length); // 1
alert(f2.length); // 2
alert(many.length); // 2
返回函数入参个数，但是...rest不参与计数

function sayHi() {
  alert("Hi");
  // 计算调用次数
  sayHi.counter++;
}
sayHi.counter = 0; // 初始值
sayHi(); // Hi
sayHi(); // Hi
alert( `Called ${sayHi.counter} times` ); // Called 2 times
可在函数内部声明一个属性，属性就是属性，不会影响函数的执行，也不会创建一个变量

let sayHi = function func(who) {
  if (who) {
    alert(`Hello, ${who}`);
  } else {
    func("Guest"); // 现在一切正常
  }
};
let welcome = sayHi;
sayHi = null;
welcome(); // Hello, Guest（嵌套调用有效）

使用 new Function 创建的函数，它的 [[Environment]] 指向全局词法环境，而不是函数所在的外部词法环境。
let func = new Function ([arg1, arg2, ...argN], functionBody);
new Function('a', 'b', 'return a + b'); // 基础语法
new Function('a,b', 'return a + b'); // 逗号分隔
new Function('a , b', 'return a + b'); // 逗号和空格分隔

计划调用(setTimeout 和 setInterval)
setTimeout 允许我们将函数推迟到一段时间间隔之后再执行。
setInterval 允许我们重复运行一个函数，从一段时间间隔之后开始运行，之后以该时间间隔连续重复运行该函数。
任何 setTimeout 都只会在当前代码执行完毕之后才会执行。
function sayHi(phrase, who) {
  alert( phrase + ', ' + who );
}
setTimeout 期望得到一个对函数的引用
setTimeout(sayHi, 1000, "Hello", "John"); // Hello, John
setTimeout(() => alert('Hello'), 1000);
取消调度的语法：
let timerId = setTimeout(...);
clearTimeout(timerId);
clearInterval(timerId)；
使用 setInterval 时，func 函数的实际调用间隔要比代码中设定的时间间隔要短！因为 func 的执行所花费的时间“消耗”了一部分间隔时间。
所以你可以使用嵌套的 setTimeout
let i = 1;
setTimeout(function run() {
  func(i++);
  setTimeout(run, 100);
}, 100);
这段代码会先输出 “Hello”，然后立即输出 “World”，这样调度可以让 func 尽快执行。但是只有在当前正在执行的脚本执行完成后，调度程序才会调用它。
setTimeout(() => alert("World"));
alert("Hello");

如果我们还希望函数立即运行，那么我们可以在单独的一行上添加一个额外的调用
function printNumbers(from, to) {
  let current = from;
  function go() {
    alert(current);
    if (current == to) {
      clearInterval(timerId);
    }
    current++;
  }
  go();
  let timerId = setInterval(go, 1000);
}
printNumbers(5, 10);



<h1>箭头函数</h1>
简洁是箭头函数的作用
function double(x) { return x * 2; } 
console.log(double(2)) // 4

const double = x => x * 2;
console.log(double(2)) // 4
如何简洁：
1.隐式返回，意味着您不需要再使用return来返回
const double = (x) => x * 2; //x为函数的参数
const getPerson = () => ({ name: "Nick", age: 24 })
2.this:
这是一个执行上下文引用的关键字，既不是指向其依赖函数本身，也不是其方法的实例，其引用的值随着上下文函数变化而改变。表面上看就是越过函数域的范围但能引用函数域外的实例并保留函数状态
function myFunc() {
  this.myVar = 0;
  setTimeout(
    () => { // this taken from surrounding, meaning myFunc here
      this.myVar++;
      console.log(this.myVar) // 1
    },
    0
  );
}
函数默认参数值
function myFunc(x = 10) {
  return x;
}
console.log(myFunc()) // 10 -- no value is provided so x default value 10 is assigned to x in myFunc
console.log(myFunc(5)) // 5 -- a value is provided so x is equal to 5 in myFunc
解构
const person = {
  firstName: "Nick",
  lastName: "Anderson",
  age: 35,
  sex: "M"
}
const arrVar = ["sad","happy",,]；
括号不用于声明对象或块，而是对象解构语法。const { age } = person;
function joinFirstLastName({ firstName, lastName }) { // we create firstName and lastName variables by destructuring person parameter
  return firstName + '-' + lastName;
}
joinFirstLastName(person); // "Nick-Anderson"

箭头函数一起使用
const joinFirstLastName = ({ firstName, lastName }) => firstName + '-' + lastName;
joinFirstLastName(person); // "Nick-Anderson"
中括号不用于声明对象或块，而是数组解构语法。
const [x, y] = arrVar；

函数式编程
函数式四骑士map,reudce,filter,find
const numbers = [0, 1, 2, 3, 4, 5, 6];
const doubledNumbers = numbers.map(n => n * 2); // [0, 2, 4, 6, 8, 10, 12]
const evenNumbers = numbers.filter(n => n % 2 === 0); // [0, 2, 4, 6]
const sum = numbers.reduce((prev, next) => prev + next, 0); // 21
const greaterThanFour = numbers.find((n) => n>4); // 5

可变参数
与golang十分相似"..."
golang中扩展arr数组使用append(...arr)
const arr1 = ["a", "b", "c"];
const arr2 = [...arr1, "d", "e", "f"]; // ["a", "b", "c", "d", "e", "f"]
你仍然可以使用arr.lenght/arr.map/arr.reduce/arr.find
arguments函数参数对象的绑定，即参数对象
这是一个可变参数和解构的例子
const myObj = { x: 1, y: 2, a: 3, b: 4 };
const { x, y, ...z } = myObj; // object destructuring here
console.log(x); // 1
console.log(y); // 2
console.log(z); // { a: 3, b: 4 }

// z is the rest of the object destructured: myObj object minus x and y properties destructured

const n = { x, y, ...z };
console.log(n); // { x: 1, y: 2, a: 3, b: 4 }

语法糖：
const x = 10;
const y = 20;

const myObj = {
  x: x, // assigning x variable value to myObj.x
  y: y // assigning y variable value to myObj.y
};

console.log(myObj.x) // 10
console.log(myObj.y) // 20

const x = 10;
const y = 20;

const myObj = {
  x,
  y
};

console.log(myObj.x) // 10
console.log(myObj.y) // 20

异步

模板文本，你可以在主流的后端语言中看到这个特新
const name = "Nick";
`Hello ${name}, the following expression is equal to four : ${2+2}`;

function comma(strings, ...values) {
  return strings.reduce((prev, next) => {
    let value = values.shift() || [];
    value = value.join(", ");
    return prev + next + value;
  }, "");
}

const snacks = ['apples', 'bananas', 'cherries'];
comma`I like ${snacks} to snack on.`;
// "I like apples, bananas, cherries to snack on."

导包，你可以在java中看到为了省略system.out的例子
// myFile.js
import { pi, exp } from './mathConstants.js'; // Named import -- destructuring-like syntax
console.log(pi) // 3.14
console.log(exp) // 2.7
关于默认导出，每个模块只有一个默认导出。默认导出可以是函数、类、对象或其他任何内容。
export default ultimateNumber;
export default function sum(x, y) {
  return x + y;
}

async/await
虽然其与golang差了一个a，但有一点是一样，await会阻止该行的执行，在golang中使用await会使主协程发生阻塞，直到golang协程都挂起


function downToOne(n) {
  const list = [];

  for (let i = n; i > 0; --i) {
    list.push(i);
  }

  return list;
}

downToOne(5)
  //=> [ 5, 4, 3, 2, 1 ]

function product(list) {
  let product = 1;

  for (const n of list) {
    product = product * n;
  }

  return product;
}

product(downToOne(5)) // 120

    yield next().value执行句块并返回 function{return}
// Yield Example
function * idMaker() {
  var index = 0;
  while (index < 2) {
    yield index;
    index = index + 1;
  }
}

var gen = idMaker();

gen.next().value; // 0
gen.next().value; // 1
gen.next().value; // undefined


// Generator Return Example
function* yieldAndReturn() {
  yield "Y";
  return "R";
  yield "unreachable";
}

var gen = yieldAndReturn()
gen.next(); // { value: "Y", done: false }
gen.next(); // { value: "R", done: true }
gen.next(); // { value: undefined, done: true }
原型：
对象的特征，因该是就是变量属于该类型就能调用该方法无需定义方法

析构等号右侧可以是任何可迭代对象
let [a, b, c] = "abc"; // ["a", "b", "c"]
let [one, two, three] = new Set([1, 2, 3]);
let {width: w = 100, height: h = 200, title} = options;
({title, width, height} = {title: "Menu", width: 200, height: 100});

日期类型
new Date(2011, 0, 1, 0, 0, 0, 0); // 1 Jan 2011, 00:00:00
new Date(2011, 0, 1); // 同样，时分秒等均为默认值 0
超出范围的日期组件将会被自动分配。
let date = new Date(2013, 0, 32); // 32 Jan 2013 ?!?
alert(date); // ……是 1st Feb 2013!
Date.now()，//返回当前的时间戳
Date.parse(str) //方法可以从一个字符串中读取日期。
//日期可以相减，得到的是以毫秒表示的两者的差值。JavaScript 中时间戳以毫秒为单位

json：
JSON.stringify 将对象转换为 JSON。
JSON.parse 将 JSON 转换回对象。

```

