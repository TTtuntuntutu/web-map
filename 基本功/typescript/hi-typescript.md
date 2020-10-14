## 	Types

### Basic

- `boolean`、`number`、`bigint`、`string`
- `array`、`tuple`
- `Enum`
- `any`、`unkown`
- **unions、intersections**
- `void`、`null`、`undefined`
  - `void`最常见的是函数无返回值
  - `null`、`undefined`是`never`外类型的子类型
    - 不开启`--strictNullChecks`，`null`、`undefined`可以赋值给除`never`任何类型
    - 开启`--strictNullChecks`，`null`、`undefined`仅允许赋值给top type和他们各自的类型。唯一例外是`undefined`可以赋值给`void`
- `never`：是所有类型的子类型



### Object

> Each property in an object type can specify a couple of things: the  type, whether the property is optional, and whether the property can be  written to.

```typescript
// 类型别名
type Point = {
  x: number;
  y: number;
};

// interface
interface Point {
  x: number;
  y: number;
}
```



#### Generic Object Types

```typescript
interface Box<T> {
  contents: T;
}

type Box<T> = {
  contents: T;
};
```



### Array&Tuple

Array本身是一种泛型，它有两种形式：

```typescript
// 形式一
function doSomething(value: Array<string>) {
  // ...
}

// 形式二
let myArray: string[] = ["hello", "world"];

// either of these work!
doSomething(myArray);
doSomething(new Array("hello", "world"));
```

其中看形式一更接近Array本身是一种泛型：

```typescript
interface Array<T> {
  /**
   * Gets or sets the length of the array.
   */
  length: number;

  /**
   * Removes the last element from an array and returns it.
   */
  pop(): T | undefined;

  /**
   * Appends new elements to an array, and returns the new length of the array.
   */
  push(...items: T[]): number;

  // ...
}
```



Tuples：

```typescript
// optional
type Either2dOr3d = [number, number, number?];
// rest elements
type StringNumberBooleans = [string, number, ...boolean[]];
// readonly tuples
function doSomething(pair: readonly [string, number]) {
  // ...
}
```

Tuples是一个**“偷懒”** 的方式，当我不想要去定义数据名字的时候，对比一下：

```typescript
function foo(...args: [string, number, ...boolean[]]) {
  var [x, y, ...z] = args;
  // ...
}

function foo(x: string, y: number, ...z: boolean[]) {
  // ...
}
```



### Functions

- Function Type Expression：描述函数长什么样

  ```typescript
  function greeter(fn: (a: string) => void) {
    fn("Hello, World");
  }
  
  function printToConsole(s: string) {
    console.log(s);
  }
  
  greeter(printToConsole);
  ```

- Call Signatures：函数可以拥有其他属性

  ```typescript
  type DescribableFunction = {
    description: string;
    (someArg: number): boolean;
  };
  
  function doSomething(fn: DescribableFunction) {
    console.log(fn.description + " returned " + fn(6));
  }
  ```

  

#### Generic Functions

> Remember, generics are all about relating two or more values with the same type!



有哪些表现：

  - typescript在许多情况可以自动推导得到T的类型：

    ```typescript
    function firstElement<T>(arr: T[]): T {
      return arr[0];
    }
    
    // s is of type 'string'
    const s = firstElement(["a", "b", "c"]);
    // n is of type 'number'
    const n = firstElement([1, 2, 3]);
    ```

  - 在使用泛型时添加限制：

    ```typescript
    function longest<T extends { length: number }>(a: T, b: T) {
      if (a.length >= b.length) {
        return a;
      } else {
        return b;
      }
    }
    
    // longerArray is of type 'number[]'
    const longerArray = longest([1, 2], [1, 2, 3]);
    // longerString is of type 'string'
    const longerString = longest("alice", "bob");
    ```




写Generic Functions的建议

- Push Type Parameters Down：尽可能使用类型参数本身，而不是给它增加限制

  ```typescript
  function firstElement1<T>(arr: T[]) {
    return arr[0];
  }
  
  function firstElement2<T extends any[]>(arr: T) {
    return arr[0];
  }
  
  // a: number (good)
  const a = firstElement1([1, 2, 3]);
  // b: any (bad)
  const b = firstElement2([1, 2, 3]);
  ```

- Use Fewer Type Parameters + Type Parameters Should Appear Twice：一个类型参数至少关联两个值，如果没有，考虑改变：

  ```typescript
  // Good
  function filter1<T>(arr: T[], func: (arg: T) => boolean): T[] {
    return arr.filter(func);
  }
  
  // Bad: 难读
  function filter2<T, F extends (arg: T) => boolean>(arr: T[], func: F): T[] {
    return arr.filter(func);
  }
  ```



#### Optional Parameters

一般使用：

```typescript
function f(x?: number) {
  // ...
}

function f(x = 10) {
  // ...
}
```



在callback中不要使用可选参数：

```typescript
function myForEach(arr: any[], callback: (arg: any, index?: number) => void) {
  for (let i = 0; i < arr.length; i++) {
    callback(arr[i], i);
  }
}

myForEach([1, 2, 3], (a) => console.log(a));
myForEach([1, 2, 3], (a, i) => console.log(a, i.toFixed()));
	//Object is possibly 'undefined'.
```

```typescript
// 订正callback中不使用可选参数
function myForEach(arr: any[], callback: (arg: any, index: number) => void) {
  for (let i = 0; i < arr.length; i++) {
    callback(arr[i], i);
  }
}

myForEach([1, 2, 3], (a) => console.log(a));
myForEach([1, 2, 3], (a, i) => console.log(a, i.toFixed()));
```



#### Function Overloads

得满足这样的条件：

1. 多个overload cases，一个implementation case
2. implementation case是不能调用的，它必须兼容所有的overload cases

```typescript
// overload signatures
function makeDate(timestamp: number): Date;
function makeDate(m: number, d: number, y: number): Date;

// implementation signatures
function makeDate(mOrTimestamp: number, d?: number, y?: number): Date {
  if (d !== undefined && y !== undefined) {
    return new Date(y, mOrTimestamp, d);
  } else {
    return new Date(mOrTimestamp);
  }
}
const d1 = makeDate(12345678);
const d2 = makeDate(5, 5, 5);
const d3 = makeDate(1, 3);
```



写overloads建议：

> Always prefer parameters with union types instead of overloads when possible

```typescript
function len(s: string): number;
function len(arr: any[]): number;
function len(x: any) {
  return x.length;
}

len(""); // OK
len([0]); // OK
len(Math.random() > 0.5 ? "hello" : [0]);	
	//No overload matches this call.
```

还不如用一个unions：

```typescript
function len(x: any[] | string) {
  return x.length;
}
```



#### Rest Parameters and Arguments

```typescript
// rest parameters
function multiply(n: number, ...m: number[]) {
  return m.map((x) => n * x);
}
// 'a' gets value [10, 20, 30, 40]
const a = multiply(10, 1, 2, 3, 4);
```

```typescript
// rest arguments
// Inferred type is number[] -- "an array with zero or more numbers",
// not specfically two numbers
const args = [8, 5];
const angle = Math.atan2(...args);
	// Expected 2 arguments, but got 0 or more.
```



### Enum

### Literal Types

常量、有字符串、数字两种类型。

单个值的Literal Types意义不大，一般放在unions：

```typescript
function printText(s: string, alignment: "left" | "right" | "center") {
  // ...
}

interface Options {
  width: number;
}
function configure(x: Options | "auto") {
  // ...
}
```



有些情况需要用`as`手动指定一下：

```typescript
const obj = { counter: 0 };
if (someCondition) {
  obj.counter = 1;
  	//可以正常修改，可以 const obj = { counter: 0 } as const
}

const req = { url: "https://example.com", method: "GET" };
handleRequest(req.url, req.method);	
	//Argument of type 'string' is not assignable to parameter of type '"GET" | "POST"'.
	//可以 const req = { url: "https://example.com", method: "GET" as "GET" };
	//或者 handleRequest(req.url, req.method as "GET");
```



## Skills

### Type Assertion

```typescript
//语法1
const myCanvas = document.getElementById("main_canvas") as HTMLCanvasElement;

//语法2:不支持jsx、tsx
const myCanvas = <HTMLCanvasElement>document.getElementById("main_canvas");
```

### Narrowing

`typeof`：返回值 `string`、`number`、`bigint`、`boolean`、`symbol`、`undefined`、`object`、`function`

```typescript
function printAll(strs: string | string[] | null) {
  if (typeof strs === "object") {
    for (const s of strs) {
				// Object is possibly 'null'. 【这里很贴心】
      console.log(s);
    }
  } else if (typeof strs === "string") {
    console.log(strs);
  } else {
    // do nothing
  }
}
```



Truthiness narrowing：值是否是真值

```typescript
function getUsersOnlineMessage(numUsersOnline: number) {
  if (numUsersOnline) {
    return `There are ${numUsersOnline} online now!`;
  }
  return "Nobody's here. :(";
}
```



Equality narrowing：`===`、`!==`、`==`、`!=` 

```typescript
function foo(left: string | number, right: string | boolean) {
  if (left === right) {
    // We can now call any 'string' method on 'x' or 'y'.
    left.toUpperCase();
					// ^ = (parameter) left: string
    right.toLowerCase();
					//^ = (parameter) right: string
  } else {
    console.log(left);
					//^ = (parameter) left: string | number
    console.log(right);
					//^ = (parameter) right: string | boolean
  }
}
```



`instanceof`：

```
function logValue(x: Date | string) {
  if (x instanceof Date) {
    console.log(x.toUTCString());
				//  ^ = (parameter) x: Date
  } else {
    console.log(x.toUpperCase());
				//   ^ = (parameter) x: string
  }
}
```



#### 特殊情况

除了用户手动narrowing，narrowing也会发生在赋值的时候：

```typescript
let x = Math.random() < 0.5 ? 10 : "hello world!";
//  ^ = let x: string | number
x = 1;

console.log(x);
//  ^ = let x: number
x = "goodbye!";

console.log(x);
//  ^ = let x: string
```



Discriminated unions: 处理多数据情况

> When every type in a union contains a common property with literal types, TypeScript considers that to be a *discriminated union*, and can narrow out the members of the union.

```typescript
interface Square {
  kind: "square";
  size: number;
}
interface Rectangle {
  kind: "rectangle";
  width: number;
  height: number;
}
interface Circle {
  kind: "circle";
  radius: number;
}

type Shape = Square | Rectangle | Circle;

function area(s: Shape) {
  switch (s.kind) {
    case "square":
      return s.size * s.size;
    case "rectangle":
      return s.height * s.width;
    case "circle":
      return Math.PI * s.radius ** 2;
  }
}
```

## operator

### Types from Extraction

从类型或者变量值中抽取一个新的类型

- `typeof`：后接变量名、属性名，从变量值抽取类型

- `keyof`：后接对象类型，提取对象key值组成的集合

  ```typescript
  type Point = { x: number; y: number };
  type P = keyof Point;
  //   ^ = type P = "x" | "y"
  
  type Arrayish = { [n: number]: unknown };
  type A = keyof Arrayish;
  //   ^ = type A = number
  
  type Mapish = { [k: string]: boolean };
  type M = keyof Mapish;
  //   ^ = type M = string | number
  ```

- Indexed Access Types：

  ```typescript
  //从类型属性名获取类型
  type Person = { age: number; name: string; alive: boolean };
  type A = Person["age"];	//   ^ = type A = number
  type I1 = Person["age" | "name"];	//   ^ = type I1 = string | number
  type I2 = Person[keyof Person];	//   ^ = type I2 = string | number | boolean
  type AliveOrName = "alive" | "name";
  type I3 = Person[AliveOrName];	//   ^ = type I3 = string | boolean
  ```

  ```typescript
  //获取数组元素类型
  const MyArray = [
    { name: "Alice", age: 15 },
    { name: "Bob", age: 23 },
    { name: "Eve", age: 38 },
  ];
  
  type T = typeof MyArray[number];
  //   ^ = type T = {
      name: string;
      age: number;
  }
  ```



### Meta-types

conditional type: `extends`、`infer`

- distributive conditional type
- Deferred Conditional Types



distributive conditional type

```typescript
type Cat = { meows: true };
type Dog = { barks: true };
type Cheetah = { meows: true; fast: true };
type Wolf = { barks: true; howls: true };
type Animals = Cat | Dog | Cheetah | Wolf;

type ExtractDogish<A> = A extends { barks: true } ? A : never;
type Dogish = ExtractDogish<Animals>;	
// = ExtractDogish<Cat> | ExtractDogish<Dog> |
//   ExtractDogish<Cheetah> | ExtractDogish<Wolf>
// = never | Dog | never | Wolf
// = Dog | Wolf
```



Deferred Conditional Types

```typescript
type Parameters<T extends (...args: any) => any> = T extends (...args: infer P) => any ? P : never;

type ReturnType<T extends (...args: any) => any> = T extends (...args: any) => infer R ? R : any;
```



### Postfix `!`

移除`null`、`undefined`值：

```typescript
function liveDangerously(x?: number | null) {
  // No error
  console.log(x!.toFixed());
}
```

## Q&A

### interface 和 type alias(类型别名)的区别？

interface和type都可以来表达对象结构、函数。它们也都支持扩展，interface可以继承interface，也可以继承type，反过来type也一样。当然写法不一样：[types-vs-interfaces](https://www.typescriptlang.org/play?e=32#example/types-vs-interfaces)

差别在哪里：

- type alias除了表达对象、函数外，还支持表达其他类型，比如 unions、tuples、primitives，这个能力interface不具备：

  ```typescript
  // primitive
  type Name = string;
  
  // object
  type PartialPointX = { x: number; };
  type PartialPointY = { y: number; };
  
  // union
  type PartialPoint = PartialPointX | PartialPointY;
  
  // tuple
  type Data = [number, string];
  ```

- interface允许定义多次，结果取交集，而type alias这样做会报错：

  ```typescript
  // These two declarations become:
  // interface Point { x: number; y: number; }
  interface Point { x: number; }
  interface Point { y: number; }
  
  const point: Point = { x: 1, y: 2 };
  ```

- type alias内支持  `in`  操作，而 interface 不支持：

  ```typescript
  //demo1
  type Partial<T> = {
      [P in keyof T]?: T[P];
  };
  
  //demo2
  type Keys = "firstname" | "surname"
  
  type DudeType = {
    [key in Keys]: string
  }
  
  const test: DudeType = {
    firstname: "Pawel",
    surname: "Grzybek"
  }
  ```



### Structural Typing(结构类型系统) vs Nominal Typing(标明类型系统)?

[浅谈 TypeScript 类型系统](https://zhuanlan.zhihu.com/p/64446259) ：TypeScript选择的是结构类型系统

> 总体上来说面向对象型的语言更多地采用 Nominal Type System，函数式语言偏向于采用 Structure Type System，JavaScript 是一种非常独特的语言，两种编程范式兼而有之，不过近些年来主流方向是函数式，arrow function、Promise 等语言特性遍地开花，React、RxJS、Ramda 等社区方案备受推崇。TypeScript 顺势而为，解决 JavaScript 弱类型问题的同时，为函数式编程的发展狠狠地添了一把火。



结构类型系统：

> TypeScript's type system is structural, which means if the type is shaped like a duck, it's a duck. If a goose has all the same attributes as a duck, then it also is a duck.

- 简单来说，比较原始数据结构是否拥有目标数据结构的成员，如果有，就可以赋值/转换；

- 比较特殊的是函数：函数赋值时，入参可以多，不能少，但是函数返回值又回到一般情况；

  ```typescript
  let createBall = (diameter: number) => ({ diameter });
  let createSphere = (diameter: number, useInches: boolean) => {
    return { diameter: useInches ? diameter * 0.39 : diameter };
  };
  
  createSphere = createBall;
  createBall = createSphere; //Type '(diameter: number, useInches: boolean) => { diameter: number; }' is not assignable to type '(diameter: number) => { diameter: number; }'.
  
  let createRedBall = (diameter: number) => ({ diameter, color: "red" });
  
  createBall = createRedBall;
  createRedBall = createBall; //Type '(diameter: number) => { diameter: number; }' is not assignable to type '(diameter: number) => { diameter: number; color: string; }'.
  ```

- [playground-example-structural-typing](https://www.typescriptlang.org/play?q=233#example/structural-typing)



在标明类型系统内，每个类型是独一无二的，即使类型的值一样也无法赋值。

考虑到特殊的场景，在typescript中可以使用intersectional type的方式来解决：

- [playground-example-nominal-typing](https://www.typescriptlang.org/play?q=233#example/nominal-typing)



### unknown vs any?

[blog:The unknown Type in TypeScript](https://mariusschulz.com/blog/the-unknown-type-in-typescript)

> The main difference between unknown and any is that unknown is much less permissive than any: we have to do some form of checking before performing most operations on values of type unknown, whereas we don't have to do any checks before performing operations on values of type any.

`any`：逃逸仓

- 任何类型都可以赋值给  `any`
- 在 `any` 类型上允许做任何操作（操作包括赋值给其他类型），而不用提前做类型检查



正因为`any`允许任何操作，会出现类型检查ok，运行时有问题的代码，所以引入了`unknown`：

- 任何类型也都可以赋值给  `unknown` 
- 禁止在  `unknown` 类型上做任何操作（操作包括赋值给其他类型），必须提前做类型检查来缩小类型范围，才可以做具体的操作；不需要类型检查、天生允许的操作： `===` 、 `!==` 、 `==` 、 `!=` 
  - 方法一：type guard
  - 方法二：type assertion
- 上述链接有如何处理`unkown`的demo



`unknown` 与 unions、intersections：

```typescript
type UnionType1 = unknown | null;       // unknown
type UnionType2 = unknown | undefined;  // unknown
type UnionType3 = unknown | string;     // unknown
type UnionType4 = unknown | number[];   // unknown

type IntersectionType1 = unknown & null;       // null
type IntersectionType2 = unknown & undefined;  // undefined
type IntersectionType3 = unknown & string;     // string
type IntersectionType4 = unknown & number[];   // number[]
```





