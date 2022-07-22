文档对象模型（Document Object Model），简称 DOM

浏览器对象模型（Browser Object Model），简称 BOM，表示由浏览器（主机环境）提供的用于处理文档（document）之外的所有内容的其他对象。

可以使用 `for..of` 对集合进行迭代。不要尝试使用 `for..in` 来迭代集合。`for..in` 循环遍历的是所有可枚举的（enumerable）属性。集合还有一些“额外的”很少被用到的属性，通常这些属性也是我们不期望得到的：

脚本是“异步”调用的

```javascript
let promise = new Promise(function(resolve, reject) {
  // executor（生产者代码，“歌手”）
});
resolve(value) —— 如果任务成功完成并带有结果 value。
reject(error) —— 如果出现了 error，error 即为 error 对象。
```

promise函数对象里有三个属性.then.catch` 和 `.finally

```javascript
window.addEventListener('unhandledrejection', function(event) {
  // 这个事件对象有两个特殊的属性：
  alert(event.promise); // [object Promise] - 生成该全局 error 的 promise
  alert(event.reason); // Error: Whoops! - 未处理的 error 对象
});
new Promise(function() {
  throw new Error("Whoops!");
}); // 没有用来处理 error 的 catch
```

Promise.all()并行执行多个 promise，并等待所有 promise 都准备就绪。

```javascript
let urls = [
  'https://api.github.com/users/iliakan',
  'https://api.github.com/users/remy',
  'https://api.github.com/users/jeresig'
];
// 将每个 url 映射（map）到 fetch 的 promise 中
let requests = urls.map(url => fetch(url));
// Promise.all 等待所有任务都 resolved
Promise.all(requests)
  .then(responses => responses.forEach(
    response => alert(`${response.url}: ${response.status}`)
  ));
```

**如果任意一个 promise 被 reject，由 `Promise.all` 返回的 promise 就会立即 reject，并且带有的就是这个 error。**



promisification 仅适用于调用一次回调的函数进一步的调用将被忽略

```javascript
// promisify(f, true) 来获取结果数组
function promisify(f, manyArgs = false) {
  return function (...args) {
    return new Promise((resolve, reject) => {
      function callback(err, ...results) { // 我们自定义的 f 的回调
        if (err) {
          reject(err);
        } else {
          // 如果 manyArgs 被指定，则使用所有回调的结果 resolve
          resolve(manyArgs ? results : results[0]);
        }
      }

      args.push(callback);

      f.call(this, ...args);
    });
  };
}
// 用法：
f = promisify(f, true);
f(...).then(arrayOfResults => ..., err => ...);
```

当一个 promise 准备就绪时，它的 `.then/catch/finally` 处理程序（handler）就会被放入队列中：但是它们不会立即被执行。当 JavaScript 引擎执行完当前的代码，它会从队列中获取任务并执行它。

`async` 确保了函数返回一个 promise，也会将非 promise 的值包装进去。

**`async/await` 可以和 `Promise.all` 一起使用**

当我们需要同时等待多个 promise 时，我们可以用 `Promise.all` 把它们包装起来，然后使用 `await`：

```javascript
// 等待结果数组
let results = await Promise.all([
  fetch(url1),
  fetch(url2),
  ...
]);
```

当你看到 `next()` 方法，或许你已经猜到了 generator 是 [可迭代（iterable）](https://zh.javascript.info/iterable)的。（译注：`next()` 是 iterator 的必要方法）

我们可以使用 `for..of` 循环遍历它所有的值：

``` javascript
function* generateSequence() {
  yield 1;
  yield 2;
  yield 3;
}
let generator = generateSequence();
for(let value of generator) {
  alert(value); // 1，然后是 2，然后是 3
}
```

`BigInt` 是一种特殊的数字类型，它提供了对任意长度整数的支持。

创建 bigint 的方式有两种：在一个整数字面量后面加 `n` 或者调用 `BigInt` 函数，该函数从字符串、数字等中生成 bigint。

| 方法名              | 搜索方式     | 可以在元素上调用？ | 实时的？ |
| ------------------- | ------------ | ------------------ | -------- |
| `querySelector`     | CSS-selector | ✔                  | -        |
| `querySelectorAll`  | CSS-selector | ✔                  | -        |
| `getElementById`    | `id`         | -                  | -        |
| `getElementsByName` | `name`       | -                  | ✔        |

elem.attributes — 所有特性的集合

elem.hasAttribute(name)` — 检查是否存在这个特性。