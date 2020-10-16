## Lodash vs Native

### You shouldn't use lodash?

[Why You shouldn’t use lodash anymore and use pure JavaScript instead-Apr 20,2018](https://codeburst.io/why-you-shouldnt-use-lodash-anymore-and-use-pure-javascript-instead-c397df51a66) 

这篇文章抛出了3个很有蛊惑力的原因：


1. performance：作者认为Lodash性能很糟糕，远低于Native，给出了“简陋”的demo；

2. chain：链式组织代码被认为是最简洁的方式（比如JQuery）。作者认为Lodash没有这个能力，当然Native是有的；

3. weight：导入一个包的代价；

   

[Benchmarking is Hard, Yo.](https://gist.github.com/RobertFischer/574a838d94e23b35a496ecfb61e0e049)

评论里面，这篇比较详细驳斥了 performance 这个观点。原文作者提供performance的测试用例噪点太大，作者改进了之后（主要是增加了测试的次数，以及提前遍历一下数据消除优化影响），得出的结论是：

1. 整体来看，Lodash和Native差别不大，Lodash的性能结果是可接受的；
2. Lodash确实可能在有些场景比Native慢一点，但这主要是Lodash源码对数据格式有校检，这种“慢”是更安全的；



chain 这个观点。不攻自破，Lodash提供了[chain api](https://lodash.com/docs/4.17.15#chain)；



weight 这个观点。影响不大，而且Lodash支持按需导入；



### You should use lodash

使用Lodash的原因：

1. Lodash提供的有些api，Native没有直接提供，或者实现起来比较麻烦。比如 [`.has`](https://lodash.com/docs/4.17.15#has) 是支持`x.x.x`的格式的，这个能力Native没有；`drop`、`dropRight`、`dropRightWhile`、`dropWhile`用原生实现要花一点代码量；
2. they avoid boilerplate code, by providing nice functional wrappers and pretty intuitive APIs；## Lodash vs Native

### You shouldn't use lodash?

[Why You shouldn’t use lodash anymore and use pure JavaScript instead-Apr 20,2018](https://codeburst.io/why-you-shouldnt-use-lodash-anymore-and-use-pure-javascript-instead-c397df51a66) 

这篇文章抛出了3个很有蛊惑力的原因：


1. performance：作者认为Lodash性能很糟糕，远低于Native，给出了“简陋”的demo；

2. chain：链式组织代码被认为是最简洁的方式（比如JQuery）。作者认为Lodash没有这个能力，当然Native是有的；

3. weight：导入一个包的代价

   

[Benchmarking is Hard, Yo.](https://gist.github.com/RobertFischer/574a838d94e23b35a496ecfb61e0e049)

评论里面，这篇比较详细驳斥了 performance 这个观点。原文作者提供performance的测试用例噪点太大，作者改进了之后（主要是增加了测试的次数，以及提前遍历一下数据消除优化影响），得出的结论是：

1. 整体来看，Lodash和Native差别不大，Lodash的性能结果是可接受的；
2. Lodash确实可能在有些场景比Native慢一点，但这主要是Lodash源码对数据格式有校检，这种“慢”是更安全的；



chain 这个观点。不攻自破，Lodash提供了[chain api](https://lodash.com/docs/4.17.15#chain)；



weight 这个观点。影响不大，而且Lodash支持按需导入；



### You should use lodash

使用Lodash的原因：

1. Lodash提供的有些api，Native没有直接提供，或者实现起来比较麻烦。比如 [`.has`](https://lodash.com/docs/4.17.15#has) 是支持`x.x.x`的格式的，这个能力Native没有；`drop`、`dropRight`、`dropRightWhile`、`dropWhile`用原生实现要花一点代码量；
2. they avoid boilerplate code, by providing nice functional wrappers and pretty intuitive APIs；