

## vscode + ts

开箱即用：

- **IntelliSense**：智能感知 => intelligent code completion
- **Hover information**：悬浮 => 类型信息、相关文档
- **Signature help**: 函数调用时，当敲入 `(` 或者 `,` 自动展示函数相关的注释信息；快捷键 `⇧⌘Space`  手动打开we
-  **Auto import**: 超酷
- JSX and auto closing tags: 自带支持jsx，只要文件后缀名 `tsx` ，毕竟自家的
- Quick Fixes
- TypeScript Snippets
- **update imports on file move**: 当更改/删除导出内容时，在使用到这部分导出内容的地方同步信息，默认询问 



熟能生巧

- **code navigation**：
  -  `F12` : 在新页面，查看选中内容的源码定义
  -  `Alt/Opt + F12`：在当前页内嵌窗口，查看选中内容的源码定义
  -  `shift + F12` ：在当前页内嵌窗口，查看选中内容的引用
  -  `win/command+F12` ：查看interface应用/实例
- refactoring

  - extract to method or function: 抽取一段逻辑为一个独立函数，inner or global

  - extract to constant: 抽取表达式为一个constant，inner or global

  - **extract type to interface or type alias**: 把interface或者type alias抽取为一个type
- organize Imports: 整理import、移除没有使用的 => 快捷键， `shift+Opt/Alt+o` or 配置进保存时起作用



插件配置

- pescript的formatting功能：交给更专业的ESLint+Prettier

- settings.json配置

  ```json
  {  
    "editor.codeActionsOnSave": {    
      "source.fixAll.eslint": true,    
      "source.organizeImports": true  
    },  
    "typescript.format.enable": false, 
  }
  ```

- [JavaScript and TypeScript Nightly](https://marketplace.visualstudio.com/items?itemName=ms-vscode.vscode-typescript-next) ：使用 `typescript@next` 作为vscode 内置ts版本



## eslint + ts

https://www.robertcooper.me/using-eslint-and-prettier-in-a-typescript-project

1. eslintrc 配置文件
2. eslintignore 忽略文件指定
3. prettierrc 配置文件
4. .vscode/settings => 编辑器相关配置
5. package.json
   1. 脚本: lint:ts
   2. husky->pre-commit hooks->lint-staged



## tsconfig

tsconfig的配置多如牛毛，不可在文档中看死，唯有在实践中认知。

这篇文章整理不错: [理解 Typescript 配置文件](https://segmentfault.com/a/1190000013514680)



### linter checks

严格版：

```json
{
  "compilerOptions":{
    "strict": true
  }
}
```

宽松版：

```json
{
  "compilerOptions":{
    "noImplicitAny": true,
    "noImplicitThis": true,
    "strictNullChecks": true
  }
}
```

