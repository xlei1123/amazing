1. 设置某个区块撑满整个屏幕： 常见背景色或者背景图，由于现在很多的应用是单页面应用，不能给body设置100% 然后子元素继承
```css
width: 100vh;
height: 100vh;
```
2. 折叠展开的需求：折叠的最小高度我们知道，但是展开的高度由于内容可能是从后端接口获取， 
[折叠展开的需求](./collapsed.html)
这里要说明的是 [css-transitions] Transition to height (or width) "auto" transition会失效，https://github.com/w3c/csswg-drafts/issues/626
3. 

