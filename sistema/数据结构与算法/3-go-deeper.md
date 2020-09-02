##  Linked List

链表的优劣往往是和数组比较的。

优势：

1. 首末尾的插入和删除操作 O(1) => **链表被用来实现栈、队列**

劣势：

1. 任意位置的插入和删除，因为首先要进行access操作，时间复杂度是 O(n) ，和数组是一样的；
2. 获取、搜索元素，时间复杂度是 O(n)，因为它需要遍历链表才能拿到地址，而数组可以直接拿到链接，时间复杂度是 O(1)；
3. 数组有更好的 cache locality([What is meant by cache locality of arrays](https://www.quora.com/What-is-meant-by-cache-locality-of-arrays))，这帮助在同样知道地址的情况下，数组能更快拿到地址对应的数据。


## Graph

图在数学中的表示：

- 图分无向图和有向图，这里是无向图

![graph数学表达](/immagine/graph数学表达.png)



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

