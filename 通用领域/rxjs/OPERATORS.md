## Operators分类

### Join Operators

#### Join Creation Operators

合并、创建 Observables。

**`combineLatest`** ：每当input Observable有值冒出的时候 => 取每个input Observable最新值组合成，冒出的值；

**`merge`** ：冒出每个 input Observable的值，和👆的区别是，冒出的是单个值，不是组合值；

**`partition`**：把一个input Observable划分成两个Observable；

`zip` ；

`concat` ：合并，前面的input Observable冒完了才到后面的input Observable；

`forkJoin` ：全部input Observable完成值时，才冒出值；

`race` ：竞速，第一个是结果；

#### Join Operators

Join Operators仅有“合并Observable”的概念，没有“创建Observable”的概念。

`concatAll`、`mergeAll`、`switchAll`、`exhaust`、`combineAll`  处理的场景叫做 Higher-order Observables，即在处理Observables的时候，又返回了Observables，比如：

```javascript
import { fromEvent, interval } from 'rxjs';
import { map, take, concatAll } from 'rxjs/operators';
 
const clicks = fromEvent(document, 'click');
const higherOrder = clicks.pipe(
  map(ev => interval(1000).pipe(take(4))),	//这里返回了Observables
);
const firstOrder = higherOrder.pipe(concatAll());
firstOrder.subscribe(x => console.log(x));
```

所以有了*outer Observable* 和 *inner Observables* 的概念。这几个api把一个high-order observable转为普通的observable，叫做 *flattening* 。在对待 outer Observable 和 inner Observable 有些差异：

- `concatAll`：订阅的是inner Observables：inner1、inner2、inner3...，inner1数据冒完了再是inner2，等等，🈶️一个**严格的顺序**；
- `mergeAll`：订阅的也是inner Observables：inner1、inner2、inner3...，无论哪个inner Observable冒出数据，冒出的数据就是它，**大家公平竞争**；
- `switchAll`：订阅的也是inner Observables：inner1、inner2、inner3...，本来inner1在开开心心地冒数据，这时候inner2来了，inner1就被抛弃了，**很喜新厌旧**；
- `exhaust`：订阅的也是inner Observables：inner1... 但是在inner1冒数据期间，产生的inner Observables不会起作用；
- `combineAll`：这位和前几位不太一样，它在等待 outer Observable 完成的同时，在收集inner Observables，等outer Observable完成的时候，通过`combineLatest`策略冒值；

另外还有两个api：`startWith`、`withLatestFrom`，它们不处理Higher-order Observables，仅仅 合并 普通 Observables。



另外有几个语法糖的api：`concatMap`、`mergeMap`、`switchMap`、`exhaustMap`。比如`concatMap`其实等于`map`+`concatAll`，其他也类似。



### Error Handling Operators

捕获错误 => 重试、抛出新的错误、抛出其他Observable