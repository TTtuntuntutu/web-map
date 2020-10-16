FP 有哪些特点

价值、哪些优点：easy to read, debug and refactor

哪些缺点

FP 的实践



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
//循环实现
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



关于递归还有一个尾部调用优化（Tail Call Optimiztion/TCO），但貌似不太明确。