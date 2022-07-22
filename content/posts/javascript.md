比较

```javascript
search 和 indexOf match //同样可用于数组
```

```javascript
substr //索引切片 数组为slice
trim //删除字符串开头或结尾的尾随空格。
includes //
concat //连接两个数组，也可以连接字符串，对然属于两个不同的对象
startsWith //ends
repeat//
```

```javascript
parseInt（）//
parseFloat（）//
Number（）//都可以用
```

```javascript
? : //三元运算符
confirm //单击“确定”将生成 true 值，而单击“取消”按钮将生成 false 值
```

```javascript
fill //数组填充
Array() //新建数组
isArray //检查数据类型是否为数组
join //连接数组元素
splice //
```

```javascript
foreach //arr.forEach(func) —— forEach 对每个数组元素都执行 func
//通常用来遍历数组，不会返回一个新数组
//Map也可以使用
map //可以返回一个新数组，方法和foreach一样，接受callback函数，其实就是有返回值的普通函数
filter find//可以返回一个新数组，方法和foreach一样，接受callback函数，其实就是有返回值的普通函数,//返回满足条件的第一个元素find( (name) => name > 1)
reduce //const sum = numbers.reduce((acc, cur) => acc + cur, 0) 0表示起始计算值
every some//检查所有元素在一个方面是否相似。它返回布尔值every((name) => typeof name === 'string')，而some检查某个元素在一个方面是否相似
```

```javascript
countries = [
  ['Finland', 'Helsinki'],
  ['Sweden', 'Stockholm'],
  ['Norway', 'Oslo'],
]
const map = new Map(countries)

set //add,delete,has,clear
map //set,get,has
```

```javascript
//解构
const countries = [['Finland', 'Helsinki'], ['Sweden', 'Stockholm'], ['Norway', 'Oslo']]
for (const [country, city] of countries) {
console.log(country, city)
}

//可以重命名
const rectangle = {
  width: 20,
  height: 10,
  area: 200
}
let { width: w, height: h, area: a, perimeter= 60} = rectangle
console.log(w, h, a, p)

//可以用展开运算符做什么
1.[gem, fra, , ...nordicCountries]
2.[...evens, ...odds]
3.{...user}
4.{...user, title:'instructor'}
```

```javascript
class Student extends Person {
  saySomething() {
    console.log('I am a child of the person class')
  }
}
```

```javascript
localStorage
setItem（），getItem（），removeItem（），clear（），key（、）
```

```javascript
getElementsByTagName（）
querySelector（）//用途包含上面的方法
querySelectorAll // 
setAttribute（）//可以赋值const titles = document.querySelectorAll('h1')
//titles[3].setAttribute('class', 'title')
//titles[3].setAttribute('id', 'fourth-title')
titles[3].classList.add('title', 'header-title') //添加类
titles[3].classList.remove('title', 'header-title') //删除类
titles[3].textContent = 'Fourth Title' //添加文本
document.createElement('h1')
document.body.appendChild(title)/removeChild(list)
selectedElement.addEventListener('eventlistner', e => {
  // 事件侦听
})
button.addEventListener //点击
```

