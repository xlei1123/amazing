- 改写方法并且呈现给用户原来的信息
```js
// console.log 被改写 但是依然可以和以前一样输出
const console_log = console.log.bind(console)
console.log = function logger (data, ...args) {
  const message = util.format(data, ...args)
  // 获取调用 Logger 的地方
  const obj = {}
  Error.captureStackTrace(obj, logger)
  const trace = obj.stack.replace(obj.toString(), '')
  process.emit('log.application', { message, trace })
  return console_log(data, ...args)
}
```


- node中的常用方法 是挂在ctx上还是global上，  挂在ctx上需要每次use,性能上会有影响吗？ 挂在global上可能会被更改  


- 代码块
> 为了代码更易于阅读 我们可以直接写这样的代码块
```js
{
  console.log(123);
  console.log(345);
}
```

- 对自身的递归操作
```js
Array.Prototype.myflat = function() {
  let result = [];
  function _flat (_this) {
    // 一些递归操作
  }
  _flat(this);
  return result;
}
```

