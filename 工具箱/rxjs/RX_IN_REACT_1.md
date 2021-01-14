[Using React and Rxjs together](https://dev.to/daslaf/using-react-and-rxjs-together-5c5l)

## 观点

> By using RxJS we can express the async dynamic behavior of our state in a **more declarative fashion（声明式）**, by using **function composition** and other **fancy functional programming techniques**.

> FRP: The essence of FRP is to specify **the dynamic behavior of a value when declaring it（在定义时，数据和行为绑定）**. 



声明式：层级、逻辑更清晰；

数据和行为绑定：逻辑更聚合，无论数据是同步还是异步，绑定的行为是什么；

## 实现Rxjs in React

> RFP：在声明阶段，定义了变量的值如何随时间变化！

从RFP定义出发，找到React中最接近的`useMemo`：

```react
import * as React from 'react';

const GreetSomeone = ({ greet = 'Hello' }) => {
    const [name, setName] = React.useState('World');
  	// 数据的计算逻辑
    const greeting = React.useMemo(() => `${greet}, ${name}!`, [greet, name]);

    React.useEffect(() => {
        fetchSomeName().then(name => {
            setName(name);
        }, () => {
            setName('Mololongo');
        });
    }, []);

    return <p>{greeting}</p>;
};
```

- 缺点：这些依赖在哪里、什么时候发生变化，是同步还是异步，这些信息不直接；




RFP：

```javascript
import { combineLatest, from, of } from 'rxjs';
import { catchError, map, startWith } from 'rxjs/operators';

const greet$ = of('Hello');
const name$ = from(fetchSomeName()).pipe(
    startWith('World'),
    catchError(() => of('Mololongo')),
);

const greeting$ = combineLatest(greet$, name$).pipe(
    map(([greet, name]) => `${greet}, ${name}!`)
);

greeting$.subscribe(greeting => {
    console.log(greeting);    
});
```

- “依赖”信息更加明确；



>  `useMemo` 约等于 `useState`+ `useEffect`

1. [StaticFlow](https://codesandbox.io/s/rxjs-react-ts-playground-njyc7?file=/src/components/how-rxjs-works-in-react/StaticFlow.tsx)：基本的 `useState` + `useEffect`
2. [ReactFlow](https://codesandbox.io/s/rxjs-react-ts-playground-njyc7?file=/src/components/how-rxjs-works-in-react/ReactFlow.tsx)：`useRef` + `BehaviorSubject`
3. [OptimizeReactFlow](https://codesandbox.io/s/rxjs-react-ts-playground-njyc7?file=/src/components/how-rxjs-works-in-react/OptimizeReactFlow.tsx)：
   1. hook1：针对value不断发生变化的，构建一个Observable；
   2. hook2：代理 值 + 订阅 + 取消订阅 逻辑；





## 其他

`useMemo` 约等于 `useState`+ `useEffect`？

```react
// useMemo
const greeting = React.useMemo(() => `${greet}, ${name}!`, [greet, name]);
```

```react
// useState + useEffect
const [greeting, setGreeting] = useState(() => `${greet}, ${name}!`);

useEffect(() => {
    setGreeting(() => `${greet}, ${name}!`);
}, [greet, name]);
```

- 虽然效果差不多，但是`useMemo`在render前计算好，`useEffect`在render后起作用；



`useRef` 返回对象，在整个组件生命周期保持不变：

> Essentially, `useRef` is like a “box” that can hold a mutable value in its `.current` property.
>
> The returned object will persist for the full lifetime of the component.
>
> Conceptually, you can think of refs as similar to instance variables in a class. Unless you’re doing [lazy initialization](https://reactjs.org/docs/hooks-faq.html#how-to-create-expensive-objects-lazily), avoid setting refs during rendering — this can lead to surprising behavior. Instead, typically you want to modify refs in event handlers and effects.