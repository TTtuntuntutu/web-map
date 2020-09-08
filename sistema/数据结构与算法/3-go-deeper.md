##  Linked List

链表的优劣往往是和数组比较的。

优势：

1. 首末尾的插入和首位删除操作 O(1) => **链表被用来实现栈、队列**

劣势：

1. 其他任意位置的插入和删除，因为首先要进行access操作，时间复杂度是 O(n) ，和数组是一样的；
2. 获取、搜索元素，时间复杂度是 O(n)，因为它需要遍历链表才能拿到地址，而数组可以直接拿到链接，时间复杂度是 O(1)；
3. 数组有更好的 cache locality([What is meant by cache locality of arrays](https://www.quora.com/What-is-meant-by-cache-locality-of-arrays))，这帮助在同样知道地址的情况下，数组能更快拿到地址对应的数据。


## Doubly Linked List

双向链表和链表很相似，时间复杂度也是一样的。它相对链表的改良在于：

1. 链表的反向遍历实现很麻烦，需要从head一次次走向反向遍历的节点，而双向链表因为一个node有两个links（一个指向previous、一个指向next），反向遍历非常容易；
2. 在链表access/search某个值的时候，其实找的是包含这个值节点的前一个节点，不直观；而双向链表可以很明确地去找包含这个值的节点；

## Queue & Stack

队列和栈是**限定了操作**的**线性顺序**的抽象数据类型

## Heap

堆是一种特殊的树型数据结构。**它一定是这一层的节点从左往右排满后，才会进入下一层**。

它有Max Heap、Min Heap两种类型，要求上下层的大小关系，其中Max Heap中value(根节点)>=value(子节点)，而不要求左右节点值的顺序。Min Heap是<=的关系。

![max-heap](/Risorse/max-heap.png)

![min-heap](/Risorse/min-heap.png)

堆的实现比较复杂，它用数组作为堆的数据容器，利用数组的index来实现堆的层级结构，使得表达基于树的数据结构（节点对应的数组序号从根节点往下递增），。在插入时候要和根节点去比较大小，在删除时得分情况判断是和根节点比较、还是和子节点比较，关键的函数是 `heapifyUp` 和 `heapifyDown` 。



## Hash Table

一般数据结构，对应key的value可以直接拿到，而hash table在中间还有buckets一层。

Hash Table得处理碰撞的情况。


## Graph

图在数学中的表示：

- 图分无向图和有向图，这里是无向图

![graph数学表达](/Risorse/graph数学表达.png)



实际场景：在用图建模后，就可以用图的方法去解决问题

社交网络：每个人是一个节点，无向图

- 给Rama推荐朋友，必须是他朋友的朋友 => 等价于，找出距离Rama是2的所有节点

![图应用-社交网络](/immagine/图应用-社交网络.png)

网络爬虫：其实是图的一个遍历，无向图

![图应用-网络爬虫](/immagine/图应用-网络爬虫.png)



加权图（weighted graph）：每条边有权重加持

-  其实可以把所有图都看作加权图：普通的图就是每条边的权重是一样的

![加权图](/immagine/加权图.png)



[mycodeschool-graph representation-adjacency list](https://www.youtube.com/watch?v=k1wraWzqtvQ&index=10&list=PLLXdhg_r2hKA7DPDsunoDZ-Z769jWn4R8) ：视频讲了图在cs中的表达方式，运用大O标记的尺子对比操作的时间复杂度、空间复杂度，打开思路的是存储一个节点的链接信息可以是数组、链表、二叉搜索树！

