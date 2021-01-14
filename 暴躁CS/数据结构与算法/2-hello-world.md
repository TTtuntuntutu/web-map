从 [itsy-bitsy-data-structures](https://github.com/jamiebuilds/itsy-bitsy-data-structures) 这里开始了 hello world 级别的了解。



## 尺子：Big-O

解决问题方案好不好需要尺子去丈量，这把尺子就是 **大O符号（BIG-O NOTATION）**



尺子丈量的两个维度：

- 时间复杂度(Time complexity)：给定n个元素的 **运算总数**；
- ~~空间复杂度(Space complexity)：给定n个元素，算法运行时占用的 **总内存**；~~



在每个维度下，尺子**丈量4种行为**：Accessing、Searching、Inserting、Deleting。

举个时间复杂度的例子：

|      |  Accessing    |   Searching   |   Inserting   |   Deleting   |
| ---- | ---- | ---- | ---- | ---- |
|   Array   |   O(1)    | O(N) | O(N) | O(N) |
| Linked List | O(N) | O(N) | O(1) | O(1) |
| Binary Search Tree | O(log N) | O(log N) | O(log N) | O(log N) |



尺子丈量出来的**常见结果**：

| Name         | Notation   | How you feel when they show up at your party |
| ------------ | ---------- | -------------------------------------------- |
| Constant     | O(1)       | AWESOME!!                                    |
| Logarithmic  | O(log N)   | GREAT!                                       |
| Linear       | O(N)       | OKAY.                                        |
| Linearithmic | O(N log N) | UGH...                                       |
| Polynomial   | O(N ^ 2)   | SHITTY                                       |
| Exponential  | O(2 ^ N)   | HORRIBLE                                     |
| Factorial    | O(N!)      | WTF                                          |




## Real Example

接下来这些耳熟能详的数据结构，其实是建立在一定基础上（这个基础就是直接操作内存的）。它们更像成品，了解它们 **能解决什么问题、适合解决什么问题，优先级高于如何实现、如何评估**。



### 线性数据结构

#### Stack

栈：数据有序，后进先出

操作：末尾插入、末尾删除、末尾数据查看。

下面用List结构实：

```javascript
class Stack {
    constructor() {
        this.list = [];
        this.length = 0;
    }

  	//O(1)
    push(value) {
        this.length++;
        this.list.push(value);
    }

  	//O(1)
    pop() {
        if (this.length === 0) return;

        this.length--;
        return this.list.pop();
    }

  	//O(1)
    peek() {
        return this.list[this.length - 1];
    }
}
```

#### Queue

队列：先进先出

操作：末尾插入、首位删除、查看首位数据

```javascript
class Queue {
    constructor() {
        this.list = []
        this.length = 0
    }
	
  	//O(1)
    enqueue(value) {
        this.length++;
        this.list.push(value)
    }
	
  	//O(n)
    dequeue() {
        this.length--;
        return this.list.shift();	
    }

		//O(1)
    peek() {
        return this.list[0]
    }
}
```

- 这里使用list/array去实现Queue，因为`arr#shift`，所以`dequeue`时间复杂度是`O(n)`
- 如果使用Linked List去实现，那么`enqueue`、`dequeue`、`peek` 的时间复杂度都是`O(1)`

#### Linked List

链表：线性集合，顺序不由内存中的物理位置确定，而是每一个元素指向下一个元素



简单实现

```javascript
class LinkedList {
    constructor() {
        this.head = null;
        this.length = 0;
    }
  	//O(n)
    get(position) {
        if (position >= this.length) {
            throw new Error('Position outside of list range');
        }

        let current = this.head;

        for (let index = 0; index < position; index++) {
            current = current.next;
        }

        return current;
    }

    add(value, position) {
        let node = {
            value,
            next: null
        };

        if (position === 0) {
            node.next = this.head;
            this.head = node;

        } else {
            let prev = this.get(position - 1);
            let current = prev.next;
            node.next = current;
            prev.next = node;
        }

        this.length++;
    }


    remove(position) {
        if (!this.head) {
            throw new Error('Removing from empty list')
        }
        if (position === 0) {
            this.head = this.head.next;

        } else {
            let prev = this.get(position - 1);
            prev.next = prev.next.next;
        }

        this.length--;
    }
}
```



链表的优点在于首末尾的插入、删除操作，时间复杂度是`O(1)`；缺点是查询、访问操作，时间复杂度是`O(n)`。

所以链表**用于实现其他数据结构**，比如Stack、Queue，因为这些数据结构的操作是链表的强项。



### 树型数据结构

#### Graph

图：由节点和连接线组成，一个节点指向另一个节点



伪代码表示：

```javascript
//单个节点
Node {
  value: ...,
  lines: [(Node), (Node), ...]
}

//整个图
Graph {
	nodes: [
    Node {...},
    Node {...},
    ...
	]
}
```



简单实现：

```javascript
class Graph {
    constructor() {
        this.nodes = [];
    }

  	//O(1)
    addNode(value) {
        return this.nodes.push({
            value,
            lines: []
        });
    }

  	//O(n)
    find(value) {
        return this.nodes.find(node => {
            return node.value === value;
        });
    }

  	//O(n)
    addLine(startValue, endValue) {
        let startNode = this.find(startValue);
        let endNode = this.find(endValue);

        if (!startNode || !endNode) {
            throw new Error('Both nodes need to exist');
        }

        startNode.lines.push(endNode);
    }
}
```

#### Tree

树是一种特殊的图，这种特殊在于树是没有环的。



tree的结构：

```javascript
 Tree {
    root: {
      value: 1,
      children: [{
        value: 2,
        children: [...]
      }, {
        value: 3,
        children: [...]
      }]
    }
  }
```



tree的简单实现：

```javascript
 class Tree {
    constructor() {
        this.root = null
    }

    traverse(callback) {
        function walk(node) {
            callback(node)

            if (node.children) {
                node.children.forEach(walk)
            }
        }

        walk(this)
    }

    add(value, parentValue) {
        const newNode = {
            value,
            children: []
        }

        if (this.root === null) {
            this.root = newNode
            return
        }

        traverse(node => {
            if (node.value === parentValue) {
                node.children.push(newNode)
            }
        })
    }
}
```



#### Binary search trees

二叉搜索树：节点所有右边值比它大、节点所有左边值比它小，且值唯一



简单实现：

```javascript
class BinarySearchTree {
  constructor() {
    this.root = null;
  }

  contains(value) {
    let current = this.root;

    while (current) {

      if (value > current.value) {
        current = current.right;

      } else if (value < current.value) {
        current = current.left;

      } else {
        return true;
      }
    }

    return false;
  }

  add(value) {
    let node = {
      value: value,
      left: null,
      right: null
    };

    if (this.root === null) {
      this.root = node;
      return;
    }

    let current = this.root;

    while (true) {

      if (value > current.value) {

        if (!current.right) {
          current.right = node;
          break;
        }

        current = current.right;

      } else if (value < current.value) {

        if (!current.left) {
          current.left = node;
          break;
        }

        current = current.left;
        
      } else {
        break;
      }
    }
  }
}
```



二叉搜索树操作的时间复杂度：

![Binary-search-tree-tc](/Risorse/Binary-search-tree-tc.png)



## 小结

1. tree的 `traverse` 函数非常重要，有了它才可以做很多事情，比如AST的遍历，但归根到底，它也不过是一个递归调用函数：

   ```javascript
   class Tree {
    	traverse(callback) {
       function walk(node) {
         callback(node);
         node.children.forEach(walk);
       }
   
       walk(this.root);
     }
   }
   ```

2. (这一节没细致讨论数据结构的优缺点和适用场景，但这一点要明确) 相比于array/lis物理位置的线性数据，**链表的优点在于首末尾的插入、删除操作，而树的优点在于查询、搜索**







