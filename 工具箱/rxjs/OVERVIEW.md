# Observables

## Pull System

数据生产者（Producer）和 数据消费者（Consumber）之间的交流策略：

1. Pull：消费者 决定什么时候从 生产者 处拉取数据，而 生产者 是不知道这个时间点的（e.g. 每个JavaScript函数是一个Pull系统，函数是数据生产者，由调用函数，即数据消费者，决定什么时候取出这个数据）；
2. Push：生产者 向 消费者 发送数据，消费者做好数据响应（e.g. Promise，作为生产者，把数据resolve给注册的回调，也就是消费者）；

![image-20201229225030083](/Users/harden_ty/Library/Application Support/typora-user-images/image-20201229225030083.png)



Rxjs引入 `Observables`，也是一个Push系统。

作为数据生产者的 `Observables` ，把数据push给数据消费者的 `Observers`。



## `Observables` 简述

```javascript
import { Observable } from 'rxjs';

// 单纯的Observable，是不会生产数据的
const foo = new Observable(subscriber => {
  console.log('Hello');
  subscriber.next(42);
});

// 只有当被 subscribe 订阅后才会生产数据
foo.subscribe(x => {
  console.log(x);
});
foo.subscribe(y => {
  console.log(y);
});
```

```javascript
import { Observable } from 'rxjs';
 
const foo = new Observable(subscriber => {
  console.log('Hello');
  subscriber.next(42);
  subscriber.next(100);
  subscriber.next(200);
  setTimeout(() => {
    subscriber.next(300); // happens asynchronously
  }, 1000);
});
 
console.log('before');
foo.subscribe(x => {
  console.log(x);
});
console.log('after');

// Observable被订阅后，产生的数据即可以是同步，也可以是异步的

// log：
// "before"
// "Hello"
// 42
// 100
// 200
// "after"
// 300
```



所以官方把Observable类比成函数调用。

但是Observable可以产生多个值，且即可以是同步数据，也可以是异步数据。



> Observables are lazy Push collections of multiple values.

这里的 lazy 指的就是 Observable 被订阅后，才可以生产数据。



## `Observable` 剖析

### 创建 Observable

在实际开发中会使用 create function，比如`of`、`from`等等。但如果用构造函数 `Observable`，需要提供一个回调函数，来描述 Observable 将如何生产数据：

```javascript
import { Observable } from 'rxjs';

const observable = new Observable(function subscribe(subscriber) {
  const id = setInterval(() => {
    subscriber.next('hi')
  }, 1000);
});
```



### 订阅 Observable => Observer

上面说到了，Observable被订阅，才会去生产数据。

一般情况下，Observer会对应一个独立的，构造函数 `Observable` 调用时的描述回调函数，即独立的context。



### 执行 Observable

执行 Observable，就是在 Observable 被订阅后，执行 构造函数 `Observable` 调用时的描述回调函数。

它有 `next`、`complete`、`error` 3个方法，其中`complete`、`error`只会被调用1次，在这之后的`next`生产数据，将无效：

```javascript
import { Observable } from 'rxjs';

const observable = new Observable(function subscribe(subscriber) {
  try {
    subscriber.next(1);
    subscriber.next(2);
    subscriber.next(3);
    subscriber.complete();
  } catch (err) {
    subscriber.error(err); // delivers an error if it caught one
  }
});
```



### 消除 Observable 执行

```javascript
// 调用下api
import { from } from 'rxjs';

const observable = from([10, 20, 30]);
const subscription = observable.subscribe(x => console.log(x));
// Later:
subscription.unsubscribe();
```

```javascript
// 之所以可以实现，是在描述函数内做了类似这样的事情
const observable = new Observable(function subscribe(subscriber) {
  // Keep track of the interval resource
  const intervalId = setInterval(() => {
    subscriber.next('hi');
  }, 1000);

  // Provide a way of canceling and disposing the interval resource
  return function unsubscribe() {
    clearInterval(intervalId);
  };
});
```



# Observer

> *Observers are just objects with three callbacks, one for each type of notification that an Observable may deliver.*

接受来自 Observable 描述函数抛出的3种类型值：

```javascript
const observer1 = {
  next: x => console.log('Observer got a next value: ' + x),
  error: err => console.error('Observer got an error: ' + err),
  complete: () => console.log('Observer got a complete notification'),
};
```



# Subscription

> *A Subscription essentially just has an `unsubscribe()` function to release resources or cancel Observable executions.*



# Operators

> RxJS is mostly useful for its *operators*, even though the Observable is the foundation. Operators are the essential pieces that **allow complex asynchronous code to be easily composed in a declarative manner.**

Observables -> Observer -> Subscription 是Rxjs的核心，而operators是构建在此之上的工具箱。

operators 分两类：

1. Pipeable Operator：允许被`source$.pipe(op1(),op2())`，不修改 input Observable，返回一个新的 output Observable，订阅 output Observable，也是订阅 input Observable；
2. Creation Operator：创建 Observable；

# Subject

> *A Subject is like an Observable, but can **multicast** to many Observers. Subjects are like EventEmitters: they maintain a registry of many listeners.*

1. 每个`Subject`是一个 `Observable`，所以可以正常订阅它。但是从 `Observer` 的角度，是不知道是 `Subject`，还是一般`Observable`；
2. 每个`Subject`也是一个 `Observer`：Observable A => Subject => Multicast Observable，作为单播转为多播的媒介；



真实使用：

- 原生的subject；

- multicast：加了一个开关的subject，开关需要手动 `multicasted.connect`；

- multicast + opertator:refCounted：开关自动打开、关闭，根据订阅的observer数目；



变体：

- BehaviorSubject：存储最新值，每当一个新的订阅，它会被最新值触发一次；
- RelaySubject：存储一串值（和BehaviorSubject类似，length可指定，还可以设置存储过期时间），每当一个新的订阅，它会被这串值触发；
- AsyncSubject：只会被最后的值触发；

