## Map

1. 测试/自动化工具，在某些方面可以帮助项目开发，它的实践是怎么样的；
2. 它的边界在哪里，相应的业界工具的情况；
3. 它未来的方向：Machine learning-based testing、 Visual AI testing
4. 背后实现的原理



## An overview of JavaScript

[An Overview of JavaScript Testing in 2020](https://medium.com/welldone-software/an-overview-of-javascript-testing-7ce7298b9870)

### terms

#### Test Types

- 单元测试(Unit Tests): input => output是否如预期，单元比如函数、类；

  ```javascript
  expect(fn(5)).to.be(10)
  ```

- 集成测试(Integration Tests): 测试过程（可能包含多个单元测试），目标和副作用；

  ```javascript
  const flyDroneButton = document.getElementById('fly-drone-button')
  
  flyDroneButton.click()
  
  assert(isDroneFlyingCommandSent())
  
  //or even
  drone.checkIfFlyingViaBluetooth()
    .then(isFlying => assert(isFlying))
  ```

- 端到端测试/功能测试(End-to-end Tests/functional tests/browser testing): 通过控制网站或者浏览器，做产品场景的功能性测试，软件是一个黑盒；

  ```javascript
  Go to page "https://localhost:3303"
  
  Type "test-user" in the field "#username"
  
  Type "test-pass" in the field "#password"
  
  Click on "#login"
  
  Expect Page Url to be https://localhost:3303/dashboard
  
  Expect "#name" to be "test-name"
  ```



补充：[What are Unit Testing, Integration Testing and Functional Testing](https://codeutopia.net/blog/2015/04/11/what-are-unit-testing-integration-testing-and-functional-testing/)

单测虽然简单，但是最常见、最主要，帮助很大：

> Code that’s hard to unit test usually has poor design.
>
>  A good set of unit tests do not only prevent bugs, but also improve  your code design, and make sure you can later refactor your code without everything completely breaking apart.



集成测试，只有当单测不够的时候，比较罕见：

> how parts of the system work together
>
> Integration testing is mainly useful for situations where unit testing  is not enough. Sometimes you need to have tests to verify that two  separate systems – like a database and your app – work together  correctly, and that calls for an integration test. 

功能测试成本更大，更加罕见，在实现过程中**没有必要特别细粒度**，测用户主要交互即可。



补充：[What’s the difference between Unit Testing, TDD and BDD?](https://codeutopia.net/blog/2015/03/01/unit-testing-tdd-and-bdd/)

**TDD是一种方法论，BDD是一种思想。**

> Unit Testing gives you the what. Test-Driven Development gives you the  when. Behavior Driven-Development gives you the how. Although you can  use each individually, you should combine them for best results as they  complement each other very nicely.



TDD(Test-Driven Development)，测试驱动开发：

> You have to write your tests before writing code.
>
> 1. Start by writing a test
> 2. Run the test and any other tests. At this point, your newly added  test should fail. If it doesn’t fail here, it might not be testing the  right thing and thus has a bug in it.
> 3. Write the minimum amount of code required to make the test pass
> 4. Run the tests to check the new test passes
> 5. Optionally refactor your code
> 6. Repeat from 1

BDD(Behavior-Driven Development)，BDD可能是最大的困惑点，以前看到文章说BDD和TDD是一样的❌：

```javascript
//without BDD
suite('Counter', function() {
  test('tick increases count to 1', function() {
    var counter = new Counter();
 
    counter.tick();
 
    assert.equal(counter.count, 1);
  });
});

//with BDD
describe('Counter', function() {
  it('should increase count by 1 after calling tick', function() {
    var counter = new Counter();
    var expectedCount = counter.count + 1;
 
    counter.tick();
 
    assert.equal(counter.count, expectedCount);
  });
});
```

场景：有一个`Counter`对象，调用`tick`方法后值➕1。without BDD建立在了初始值是0的基础（虽然代码里面值真的是0），但这样就依赖实现细节。如果代码发生变化，单测代码也需要发生变化。with BDD测试的是行为，与代码细节无关，避免单测代码频繁改动。



#### Running Tests

- Test can run in the browser
- Test can run in a headless browser
- Tests can also be executed in Node.js => 推荐 Node.js+jsdom(模拟浏览器环境)、快





#### Test Tools Types

> Some provide us with only one functionality, and some provide us with a combination.
>
> To achieve the most flexible set functionality, it’s common to use a combination of several tools.

- Test Launchers：user config => 在browser or Node.js中启动测试；
- Test structure:  组织测试代码、描述测试行为，`describe`、`it` 那些
- Assertion functions：断言
- Generate and display test progress and summary: 测试结果概览 
- Generate code coverage: 测试代码覆盖率

------------- 👆熟悉 -------- 👇茫然----------

- Mocks, spies and stubs: 测试部分和其他部分隔离，**模拟测试场景**，将它附加到流程中查看是否按预期工作 => 集成测试
  - spies：**监控**，添加到函数获取额外信息，函数调用次数等；
  - stubs：将选中模块下的方法用用户自定义的方法替换，比如一直返回`true` ，就达到在运行测试时，这个方法一定是如预期的，模拟需要的测试环境；
  - mocks：也叫fakes，比如模拟浏览器
- Generate and compare snapshots: 比较前后数据快照，通常用于组件表示数据 => 集成测试
- Browsers Controllers: 模拟用户操作浏览器行为 => 功能测试
- Visual Regession Tools: 功能测试



### 解决方案

测试架构：

1. 服务单测+集成测试：在编写代码时运行
2. 服务功能测试：在合并、发布前运行



### 工具推荐





### 作者

Jest满意度最高：

> According to [The State of JavaScript](http://stateofjs.com). [Jest](https://jestjs.io/), the leading unit test framework that we would discuss later in details, has it’s satisfaction rates at **96%**!



mocha+chai+sinon是流行的组合



未来展望：AI =>  automated testing 

> Looking forward, I forecast a large entrance of AI into the field of  automated testing. Some such tools already exist, and actively improving the workflow and experience of thousands of developers.



要想单测写得好，函数式编程、纯函数少不了：

> **Unit tests are one of the reasons to use functional programming and pure functions as much as possible.**
>
> **The purer your application is, the easier you can test it.**

现实中unit写太烂，单测干不了，集成测试来补：

> It’s also important to remember that **in the real world**, for the reasons of imperfect design and the widespread use of black  boxes, not all units are pure and not all units are testable- some units can be tested only as part of a bigger process.



unit tests

- 要求：Assertion functions +  Code coverage reporting tool
  - so，建议写functional programming and pure functions，因为更容易测试

integration tests

- 要求：Mocks, spies and stubs + snapshots



1. 查询合同结果不对；
2. 查询签署详情接口变化，不使用flowId；
3. 取消签署接口没录入；
4. 合同详情返回的电子签署记录数据没有标明清楚；