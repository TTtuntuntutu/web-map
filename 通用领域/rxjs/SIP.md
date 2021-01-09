[The SIP principle](https://blog.strongbrew.io/the-sip-principle/)：**Everything is a stream!**

- S: Source streams，用户交互行为
  - 命名细节：以`$`结尾，关注交互行为包含的数据（比如input搜索，取名为`searchTerm$`而不是`search$`）；
  - 一般是 Subjects（即多播）；
- P: Presentation streams，**组件要渲染的动态数据**；
- I: Intermediate streams，S 和 P 之间的桥梁

