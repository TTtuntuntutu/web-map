## Rxjs简单尝试感受

仅仅是计算逻辑的场景：

- useMemo也可以解决，虽然Rxjs（lodash for events）层次清晰，可以有创造力地编排，但是优势不明显，成本 > 优点；

简单的异步请求的场景：

- 相对于传统的useState+useMemo实现，rxjs对于数据和数据行为的绑定（数据来源、数据处理）更密切，但是该做的事情是一样的，而且可以提取hook来使得这部分逻辑去除重复。有优势，但还不够；

弹窗的确定按钮点击行为 => 校检行为（过滤校检不通过） => 表单数据异步提交场景：

- 行为的先后顺序层次清晰，可以明白由一个点出发，有哪些关联的行为。但是相应的用hooks去提取这段逻辑，也不难实现；

首次异步请求+后续主动可以调用相同的异步请求逻辑：

- rxjs的方式和一般的处理（useEffect+主动调用逻辑函数）没有优势；



简单场景下，rxjs的优势其实就是function compose的层次清晰，通过传统方法+hooks逻辑提取，同样可以做到干净、且认知便利。

