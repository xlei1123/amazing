1. 改写方法并且呈现给用户原来的信息
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


2. node中的常用方法 是挂在ctx上还是global上，  挂在ctx上需要每次use,性能上会有影响吗？ 挂在global上可能会被更改  


3. 代码块
> 为了代码更易于阅读 我们可以直接写这样的代码块
```js
{
  console.log(123);
  console.log(345);
}
```

4. 对自身的递归操作
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

5. 表驱动改善if/switch

demo1:
代码改善前：
```js
const day = new Date().getDay();
let day_zh;
if(day === 0){
    day_zh = '星期日'
}else if(day === 1) {
    day_zh = '星期一'
}
...
else{
    day_zh = '星期六'
}

// 或者 用 switch case
switch(day) {
  case 0:
    day_zh = '星期日'
    break;
  case 1:
    day_zh = '星期一'
    break;
    ...
}

```
表驱动改善后：
```js
const week = ['星期日', '星期一',..., '星期六'];
const day = new Date().getDay();
const day_zh = week[day];
```

demo2: 考试按分数评优良中差四个等级

```js
  let level = '优'
  if(score <60 ){
    level = '差'
  }else if(score < 80) {
    level = '中'
  }else if(score < 90) {
    level = '良'
  }

```

如果需要添加等级 60-70 之间 是一般，就需要改逻辑，逻辑简单的还好

```js
  let level = '优';
  if(score <60 ){
    level = '差'
  }else if(score < 70) {
    level = '一般'
  }else if(score < 80) {
    level = '中'
  }else if(score < 90) {
    level = '良'
  }

```

表驱动改善后：用阶梯查询就比较方便，而且扩展性也好

```js
  const levelTable = ['差', '中', '良', '优'];
  const scoreCeilTable = [60, 80, 90, 100];
  
  function getLevel(score) {
    const len = scoreCeilTable.length
    for(let i = 0; i < len; i++) {
      const scoreCeil = scoreCeilTable[i]
      if(score <= scoreCeil) {
        return levelTable[i]
      }
        
    }
    return levelTable[len - 1];
  }

```

就算后面需要添加等级60-70 ---> '一般' 也只需要简单添加数据表就可以了，而不需要修改逻辑

```js
  const levelTable = ['差', '一般', '中', '良', '优'];
  const scoreCeilTable = [60, 70, 80, 90, 100];

```
6. 函数传参
常见有多个参数，对于需要传递函数的 我们经常会传递一个什么都不执行的函数() => {}, null等，下面这种提供了另外一种思路：
```js
 // 定义一个person函数
 function person(name,age,say){
   if(typeof age === 'function'){ // 重点在这里
      say = age;
   }
   say && say();
 }
 // 调用方法一
 person('Bryson',22,function(){
   console.log('我是Bryson啊')
 })
 // Bryson 22
 // 我是Bryson啊
 ​
 // 调用方法二，不传age，让function()直接切换给age
 person('Bryson',function(){
   console.log('我是Bryson啊')
 })
 // Bryson f(){console.log('我是Bryson啊')}
 // 我是Bryson啊

```

