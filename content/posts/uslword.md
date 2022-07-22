常用的javascript关键词

```javascript
数据相关的:
toString()
toFixed()//将数字舍入到小数点后 n 位，并以字符串形式返回结果
alert( 6.35.toFixed(1) ); // 6.3
//由于精度问题你可以先使用round缩减至1个精度再扩大精度
alert( num.toString(2) );   // 11111111
其值大小最小2进制最大36进制
isNaN(value)//值 “NaN” 是独一无二的，它不等于任何东西，包括它自身,该函数会先转换成数然后再比较是否为0
Object.is(NaN，NaN)//类似于===但可以比较NaN
isFinite(value)
parseInt()//和+号区别就是只要按顺序读字符串只要不是非字符串就能转换，+就严格许多
parseFloat()
一般小数基本会出现精度损失
Math.floor
Math.ceil
Math.round
Math.trunc

<h1>字符串相关</h1>
//反引号允许我们通过 ${…} 将任何表达式嵌入到字符串中
.length
str.charAt() //获取在位置的一个字符,可以使用 for..of 遍历字符
str.indexOf(substr, pos)//给定位置 pos 开始，在 str 中查找 substr
//JavaScript 中有三种获取字符串的方法：substring、substr 和 slice
alert( 'Interface'.toUpperCase() ); // INTERFACE
alert( 'Interface'.toLowerCase() ); // interface
indexOf()//如果没有找到，则返回 -1，否则返回匹配成功的位置
(~str.indexOf(...))//对于 32-bit 整数，~n 等于 -(n+1)，所以你可以这么用if 
str.includes(substr, pos) 还有 startsWith，endsWith//根据 str 中是否包含 substr 来返回 true/false
slice(start, end)//从 start 到 end（不含 end）允许负值,足够用
小写字母总是大于大写字母，可以使用str.codePointAt(pos)返回在 pos 位置的字符代码 
alert( 'Österreich'.localeCompare('Zealand') ); // -1
获取字符时，使用 []
str.trim() —— 删除字符串前后的空格 (“trims”)
str.repeat(n) —— 重复字符串 n 次

数组
在golang中数组是值，切片或许和javascript数组最类似
let fruits = ["Apple", "Orange", "Plum"];
arr.at(-1)//参数可为负值
push //在末端添加一个元素.
shift //取出队列首端的一个元素，整个队列往前移，这样原先排第二的元素现在排在了第一。
push //在末端添加一个元素.
pop //从末端取出一个元素.
unshift//在数组的首端添加元素
请不要：
添加一个非数字的属性，比如 arr.test = 5。
制造空洞，比如：添加 arr[0]，然后添加 arr[1000] (它们中间什么都没有)。
以倒序填充数组，比如 arr[1000]，arr[999] 等等。
这会破坏数组的有序储存结构
for..in 循环适用于普通对象，并且做了对应的优化。用于数组速度要慢 10-100 倍。当然即使是这样也依然非常快。只有在遇到瓶颈时可能会有问题。但是我们仍然应该了解这其中的不同。
arr.length = 2; // 截断到只剩 2 个元素
alert( arr ); // [1, 2]
arr.length = 5; // 又把 length 加回来
alert( arr[3] ); // undefined：被截断的那些数值并没有回来
如果使用 == 来比较数组，除非我们比较的是两个引用同一数组的变量，否则它们永远不相等。

对象相关
Object.keys(obj) —— 返回一个包含该对象所有的键的数组。
Object.values(obj) —— 返回一个包含该对象所有的值的数组。
Object.entries(obj) —— 返回一个包含该对象所有 [key, value] 键值对的数组。
字符串类型，symbol类型可以用作对象属性键，其他不行
symbol() //保证是唯一的。即使我们创建了许多具有相同描述的 symbol，它们的值也是不同。描述只是一个标签
let user = {
  name: "John",
  [Symbol("id")]: 123 // 要在对象字面量 {...} 中使用 symbol，则需要使用方括号把它括起来
};
Object.keys(user) 会忽略symbol值，但Object.assign 会同时复制字符串和 symbol 属性
要从全局symbol注册表中读取（不存在则创建，请使用 Symbol.for(key)，Symbol.keyFor(sym)
// 通过 name 获取 symbol
let sym = Symbol.for("name");
let sym2 = Symbol.for("id");
// 通过 symbol 获取 name
alert( Symbol.keyFor(sym) ); // name
alert( Symbol.keyFor(sym2) ); // id
Array.from()接受一个可迭代或类数组的值，并从中获取一个“真正的”数组，在这里要提一嘴类数组 
let arrayLike = {
  0: "Hello",
  1: "World",
  length: 2
};
//是的，这不是对象，也不是数组
let arr = Array.from(arrayLike); // 转换成可迭代数组
还有使用展开运算符“...”也可以将可迭代对象转换为真正的数组
let str = "𝒳😂";
let chars = [...str];
原始值转换相当于可自定义对象转换规则
Symbol.toPrimitive
toString/valueOf
let user = {
  name: "John",
  money: 1000,
  [Symbol.toPrimitive](hint) {
    alert(`hint: ${hint}`);
    return hint == "string" ? `{name: "${this.name}"}` : this.money;
  }
};


// 转换演示：
alert(user); // hint: string -> {name: "John"}
alert(+user); // hint: number -> 1000
alert(user + 500); // hint: default -> 1500

MAP，集合
Map 使用 SameValueZero 算法来比较键是否相等。它和严格等于 === 差不多，但区别是 NaN 被看成是等于 NaN。所以 NaN 也可以被用作键。
new Map() —— 创建 map。
map.set(key, value) —— 根据键存储值。
map.get(key) —— 根据键来返回值，如果 map 中不存在对应的 key，则返回 undefined。
map.has(key) —— 如果 key 存在则返回 true，否则返回 false。
map.delete(key) —— 删除指定键的值。
map.clear() —— 清空 map。
map.size —— 返回当前元素个数。
map.keys() —— 遍历并返回一个包含所有键的可迭代对象，
map.values() —— 遍历并返回一个包含所有值的可迭代对象，
map.entries() —— 遍历并返回一个包含所有实体 [key, value] 的可迭代对象，for..of 在默认情况下使用的就是这个。
从一个已有的普通对象（plain object）来创建一个 Map
let obj = {
  name: "John",
  age: 30
};
let map = new Map(Object.entries(obj));
Object.fromEntries 方法的作用是相反的：给定一个具有 [key, value] 键值对的数组，它会根据给定数组创建一个对象

WeakMap，WeakSet也与之类似
weakMap.get(key)
weakMap.set(key, value)
weakMap.delete(key)
weakMap.has(key)
用 WeakMap 替代 Map，当对象被垃圾回收时，对应缓存在WeakMap的键值对也会自动从内存中清除。适用于键中变更频繁的对象


SET “值的集合”（没有键）
new Set(iterable) —— 创建一个 set，如果提供了一个 iterable 对象（通常是数组），将会从数组里面复制值到 set 中。
set.add(value) —— 添加一个值，返回 set 本身
set.delete(value) —— 删除值，如果 value 在这个方法调用的时候存在则返回 true ，否则返回 false。
set.has(value) —— 如果 value 在 set 中，返回 true，否则返回 false。
set.clear() —— 清空 set。
set.size —— 返回元素个数。

垃圾回收
JavaScript 引擎在值“可达”和可能被使用时会将其保持在内存中，通常，当对象、数组之类的数据结构在内存中时，它们的子元素，如对象的属性、数组的元素都被认为是可达的。例如，如果把一个对象放入到数组中，那么只要这个数组存在，那么这个对象也就存在，即使没有其他对该对象的引用。
与golang相同当你将引用，或者内存共享的值：比如slice中的值删除除非你将其置为nil，否则垃圾回收器不会回收。


```



