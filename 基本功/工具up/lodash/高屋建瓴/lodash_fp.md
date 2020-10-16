## key

> lodash/fp: produce immutable auto-curried iteratee-first data-last methods

## use

Mapping：

- Fixed Arity: Methods have fixed arities to support auto-currying. => 有>=2个参数作为前提条件
- Rearranged Arguments: Method arguments are rearranged to make composition easier. => 参数的顺序调整，使得组合更加简单
- Capped Iteratee Arguments: Iteratee arguments are capped to avoid gotchas with variadic iteratees. => 迭代函数做参时，捕获的参数和参数个数
- No Optional Arguments: Optional arguments are not supported by auto-curried methods. => 可选参数要显示传入

Placeholders：

- `_`占位符，可以去调整lodash/fp指定的顺序

Chaining：

- [Why using `_.chain` is a mistake](https://medium.com/bootstart/why-using-chain-is-a-mistake-9bc1f80d51ba)



### link

[Lodash wiki FP Guide](https://github.com/lodash/lodash/wiki/FP-Guide)

[What is Lodash/fp, even?](https://dev.to/ifarmgolems/what-is-lodash-fp-even-4ohd?signin=true)