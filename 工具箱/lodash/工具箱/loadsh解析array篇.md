## Drop组

[drop](https://lodash.com/docs/4.17.15#drop)、[dropRight](https://lodash.com/docs/4.17.15#dropRight)、[dropRightWhile](https://lodash.com/docs/4.17.15#dropRightWhile)、[dropWhile](https://lodash.com/docs/4.17.15#dropWhile) 4个api都是按一个方向（从前往后、从后往前）剔除数组元素。区别是前两个api是指定剔除的元素个数；后两个api指定一个逻辑遍历，删除逻辑通过的元素，直到遇到第一个逻辑不通过的元素，遍历就停止。

> 分析1：api有没有使用的必要性，即原生实现麻不麻烦？

```javascript

function dropDiy(arr, num){
  return arr.filter((c,i)=>i>=num) 方法2
}

function dropRightDiy(arr, num){
  return arr.filter((c,i,a)=>i<(a.length-num))
}

function dropRightWhileDiy(data, fn){
  let stop = false

  return data.reverse().filter(e => {
    if(!stop){
      const check  = fn(e)
      stop = check?false:true
      return !check
    }else{
      return true
    }
  }).reverse()
}
```

> 分析2：Lodash源码是怎么实现的，源码的闪光点

自己实现用了filter的思想，看了Lodash源码是slice的思想，关键在找分界点的序号，这更加简单：

```javascript
function dropDiy(arr, num){
  return arr.slice(num)
}

function dropRightDiy(arr, num){
  return arr.slice(0, arr.length-num)
}

function dropRightWhileDiy(arr, predicate) {
  const reverse_arr = [...arr].reverse()
  const index = reverse_arr.findIndex((v)=>!predicate(v))
  return arr.slice(index).reverse()
}

function dropWhileDiy(arr, predicate) {
  const index = arr.findIndex((v)=>!predicate(v))
  return arr.slice(index)
}
```



自己实践还比较复杂，推荐用lodash

## 命名

`predicate`：基于的逻辑，`dropWhile`、`dropRightWhile`第二个参数用了这个词

