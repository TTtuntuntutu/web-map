

## 特点

> JavaScript is a multi-paradigm language that allows you to freely mix  and match object-oriented, procedural, and functional paradigms. 



### Pure Functions

纯函数的特点：

1. 相同入参，总是返回值相同；
2. 没有副作用（比如console输出、改变对象、重新赋值等）；

```javascript
//pure
function multiply(a, b) {
  return a * b;
}

let heightRequirement = 46;

// Impure because it relies on a mutable (reassignable) variable.
function canRide(height) {
  return height >= heightRequirement;
}

// Impure because it causes a side-effect by logging to the console.
function multiply(a, b) {
  console.log('Arguments: ', a, b);
  return a * b;
}
```



建议配置：

- 项目不可能完全由纯函数组成，建议pure/impure=80/20



纯函数优点：

- 函数作用易推导、函数易debug；
- 函数返回结果可缓存：因为返回值永远不变；
- 函数易测试，不用去mock外部依赖；



识别不是纯函数Tips：

> 如果函数没有返回值，大概率就是impure functions，因为这样函数的价值很可能表现在它的副作用；



### Immutability

> Immutability pertains to all data structures, which includes arrays, maps, and sets. 

这些数据结构是引用地址，可能存在多个变量指向一个地址。如果通过一个变量，修改了引用地址里的内容。可能存在bug的面积就不好估计了。当使用Immutability时候，我的更改点就在这里，有问题也只可能在这里。



工具：[Immutable.js](https://immutable-js.github.io/immutable-js/)



### Function Composition

$(f ∘ g)(x) = f(g(x))$ 



### Recursion

> Recursion is one of the most powerful tools in the functional  programmer's toolbelt. Recursion asks us to break down the overall  problem into sub-problems that resemble the overall problem.



循环不是FP范围内。对于要用到循环的场景，可用Recursion代替。

比如求阶乘：$n! = n \* (n-1) \* (n-2) \* ... \* 1$

```javascript
//循环实现：典型的面向过程编程
function iterativeFactorial(n) {
  let product = 1;
  for (let i = 1; i <= n; i++) {
    product *= i;
  }
  return product;
}
```

```javascript
//递归实现
function recursiveFactorial(n) {
  // Base case -- stop the recursion
  if (n === 0) {
    return 1; // 0! is defined to be 1.
  }
  return n * recursiveFactorial(n - 1);
}
```



递归的思想有两步：

1. 原问题拆成子问题，子问题和愿问题是类似的：$n! = n * (n-1)!$
2. 找到停止条件



关于递归还有一个话题：尾部调用优化（Tail Call Optimiztion/TCO），它对递归性能做了优化。
但是它似乎经历了复杂的过程后走向了死亡：[Tail Call Optimization implementation in Javascript Engines](https://stackoverflow.com/questions/54719548/tail-call-optimization-implementation-in-javascript-engines)


### Higher-order functions

 return a function from a function



### Curring

native有原生的机制：

```javascript
function giveMe3(item1, item2, item3) {
  return `
    1: ${item1}
    2: ${item2}
    3: ${item3}
  `;
}

const giveMe2 = giveMe3.bind(null, 'rock');
const giveMe1 = giveMe2.bind(null, 'paper');
const result = giveMe1('scissors');

console.log(result);
// 1: rock
// 2: paper
// 3: scissors
```



## 个人总结

对FP的这些特点做一个抽象，可以总结为“一个方块的故事”。

![](/assets/FP抽象.jpeg)

如果说面向过程编程是傻瓜式的暴力求解，面向对象编程是对现实世界真实的建模，那么函数式编程，是用抽象的思维找到干净的求解规律。



优点/价值：easy to read, debug and refactor

实践：Lodash/fp、ramda



## 链接

[An introduction to functional programming in JavaScript](https://opensource.com/article/17/6/functional-javascript)


