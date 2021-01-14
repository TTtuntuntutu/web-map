## up

1. static typechecker：提前捕获可能的错误

   > The goal of TypeScript is to be a static typechecker for JavaScript programs - in other words, a tool that runs before your code runs (static) and ensure that the types of the program are correct (typechecked).

2. suggesting/completion：因为有数据结构信息，所以可以做提示、代码补全

   > The type-checker has information to check things like whether we’re  accessing the right properties on variables and other properties. Once it has that information, it can also start *suggesting* which properties you might want to use.
   >
   > That means TypeScript can be leveraged for editing code too, and the  core type-checker can provide error messages and code completion as you  type in the editor. That’s part of what people often refer to when they talk about tooling  in TypeScript.

3. quick fixes、refactorings to re-organize code、 navigation features for jumping to definitions of a variable、finding all references to a given variable：编辑器提供的便利



## 成本

typescript舒缓javascript接入的曲线。

1. 哪怕static typechecker捕获错误，tsc也可以正常编译出js代码

   > on one of TypeScript’s core values: much of the time, *you* will know better than TypeScript.

2. 在许多情况不需要显示标明类型，typescript可以推导

