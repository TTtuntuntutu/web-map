

## Jest

测试框架：单测、集成测试  by Facebook

提供以下能力：

- Test Launchers: 测试运行环境
- Test structure
- assertions、spies、mocks => 类似Sinon
- snapshot testing => jest-snapshot



### details

#### Test Structure

`describe?` => `test` => `expect`

```javascript
describe('matching cities to foods', () => {
  // Applies only to tests in this describe block
  beforeEach(() => {
    return initializeFoodDatabase();
  });

  test('Vienna <3 sausage', () => {
    expect(isValidCityFoodPair('Vienna', 'Wiener Schnitzel')).toBe(true);
  });

  test('San Juan <3 plantains', () => {
    expect(isValidCityFoodPair('San Juan', 'Mofongo')).toBe(true);
  });
});
```



#### 使用Matchers

这就是断言（assertions）。`expect(...)` 是 expectation，`toBe(..)` 是一种 matcher。Jest提供了许多matchers，不匹配的最终被Jest摘出来；

```javascript
test('two plus two is four', () => {
  expect(2 + 2).toBe(4);
});
```

#### Mock Functions

Mock函数、模块、结果，同时可获取函数调用等信息

1. 创建一个Mock函数（自己写，比如有些需要传入一个callback），有一个`.mock` 属性可以拿这个函数被调用信息、`.mockReturnValue`、`.mockReturnValueOnce` 设置函数返回值等；

2. 手动mock(manaul mock)

   - 模拟模块，比如axios：

     ```javascript
     // users.js
     import axios from 'axios';
     
     class Users {
       static all() {
         return axios.get('/users.json').then(resp => resp.data);
       }
     }
     
     export default Users;
     
     // users.test.js
     import axios from 'axios';
     import Users from './users';
     
     jest.mock('axios');
     
     test('should fetch users', () => {
       const users = [{name: 'Bob'}];
       const resp = {data: users};
       axios.get.mockResolvedValue(resp);
     
       // or you could use the following depending on your use case:
       // axios.get.mockImplementation(() => Promise.resolve(resp))
     
       return Users.all().then(data => expect(data).toEqual(users));
     });
     ```

   - 模拟结果(mock implementation)

#### 勾子/预准备

**在开始之前、结束之后的钩子函数**：比如数据库的初始化和结束时的清理；

```javascript
beforeEach(() => {
  initializeCityDatabase();
});

afterEach(() => {
  clearCityDatabase();
});

test('city database has Vienna', () => {
  expect(isCity('Vienna')).toBeTruthy();
});

test('city database has San Juan', () => {
  expect(isCity('San Juan')).toBeTruthy();
});
```



预准备可通过配置jest.config.js `setupFilesAfterEnv`实现



#### snapshot

UI测试，比如测试一个组件，变化是否如预期

```tsx
import React from 'react';
import renderer from 'react-test-renderer';
import Link from '../Link.react';

it('renders correctly', () => {
  const tree = renderer
    .create(<Link page="http://www.facebook.com">Facebook</Link>)
    .toJSON();
  expect(tree).toMatchSnapshot();
});
```

更新

- 所有：`jest --updateSnapshot `

- 指定更新snap：`--testNamePattern`

其他

- 把snapshots当作代码提交

- 在不修改组件时，多次跑生成snapshots结果是一样的，如果有特定的数据，要提前mock

  ```javascript
  Date.now = jest.fn(() => 1482363367071);
  ```

  





## DOM Tesing: Jest + Testing Library

参考链接：[How to use React Testing Library Tutorial](https://www.robinwieruch.de/react-testing-library)



### 初始化

Jest是一个测试框架，提供了运行测试的环境和开箱即用的功能。

Testing Library提供了模拟用户测试UI、断言DOM的能力，在测试领域某些功能做了扩展（`@testing-library/user-event`、`@testing-library/jest-dom`）



以 Jest+Typescript+React Testing Library为例

- 安装：jest、jest-transform-stub、ts-jest、~~react-test-renderer~~、@testing-library/jest-dom、@testing-library/react、@testing-library/user-event、@types/jest、@types/react-test-renderer

- 配置jest.config.js：

  ```javascript
  module.exports = {
    testEnvironment: 'jsdom',
    testMatch: ['...'],
    transform: {
      '^.+\\.(ts|tsx)$': 'ts-jest',
    },
    moduleNameMapper: {
      '^.+.(css|styl|less|sass|scss|png|jpg|ttf|woff|woff2)$': 'jest-transform-stub',
    },
    setupFilesAfterEnv: ['./jest.setup.js'],
  }
  ```

- 有必要添加jest.setup.js：

  ```javascript
  // antd存在问题
  Object.defineProperty(window, 'matchMedia', {
    writable: true,
    value: jest.fn().mockImplementation((query) => ({
      matches: false,
      media: query,
      onchange: null,
      addListener: jest.fn(), // deprecated
      removeListener: jest.fn(), // deprecated
      addEventListener: jest.fn(),
      removeEventListener: jest.fn(),
      dispatchEvent: jest.fn(),
    })),
  })
  ```



### 开始

#### grab elements

getBy：returns an element or an error

- **getByText**：可见的文案
- **getByRole**：accessibility role，正确使用html标签很重要哈（默认带上正确的role），传入不存在role值时，会展示所有可选role值
- getByLabelText：`<label for="search" />`
- getByPlaceholderText：`<input placeholder="Search" />`
- getByAltText：`<img alt="profile" />`
- getByDisplayValue：`<input value="JavaScript" />`
- getByTestId：需要手动给元素加一个`data-testid` 属性 （一般❌，因为要动原文件）



queryBy、findBy 同样有这样7个api:

> For any element that isn't there yet but will be there eventually, use  findBy over getBy or queryBy. If you assert for a missing element, use  queryBy. Otherwise default to getBy.

getBy、queryBy、findBy仅返回一个，getAllBy、queryAllBy、findAllBy返回一个数组



#### assertive functions

大多数的断言函数来自jest，React Testing Library扩展了一些，比如`toBeNull`、`toBeInTheDocument`，需要安装并导入：[@testing-library/jest-dom](https://github.com/testing-library/jest-dom)



#### fire events

基础的fireEvent Api:

```tsx
import {screen, fireEvent } from '@testing-library/react';

describe('',()=>{
   fireEvent.change(screen.getByRole('textbox'), {
      target: { value: 'JavaScript' },
   });
})
```



React Testing Library在此基础上做了`'@testing-library/user-event'` ，比fireEvent Api更加方便的userEvent，作者是推荐这个工具的。



#### Callback Handlers

使用jest Mock Functions，单独拎出React组件，单测：

```tsx
//Search组件
function Search({ value, onChange, children }) {
  return (
    <div>
      <label htmlFor="search">{children}</label>
      <input
        id="search"
        type="text"
        value={value}
        onChange={onChange}
      />
    </div>
  );
}
```

```tsx
//Search测试用例
describe('Search', () => {
  test('calls the onChange callback handler', async () => {
    const onChange = jest.fn();
 
    render(
      <Search value="" onChange={onChange}>
        Search:
      </Search>
    );
 
    await userEvent.type(screen.getByRole('textbox'), 'JavaScript');
 
    expect(onChange).toHaveBeenCalledTimes(10);
  });
});
```



#### Async

使用jest Mock Functions，mock Axios，断言UI前后的变化：

```tsx
//组件
import React from 'react';
import axios from 'axios';
 
const URL = 'http://hn.algolia.com/api/v1/search';
 
function App() {
  const [stories, setStories] = React.useState([]);
  const [error, setError] = React.useState(null);
 
  async function handleFetch(event) {
    let result;
 
    try {
      result = await axios.get(`${URL}?query=React`);
 
      setStories(result.data.hits);
    } catch (error) {
      setError(error);
    }
  }
 
  return (
    <div>
      <button type="button" onClick={handleFetch}>
        Fetch Stories
      </button>
 
      {error && <span>Something went wrong ...</span>}
 
      <ul>
        {stories.map((story) => (
          <li key={story.objectID}>
            <a href={story.url}>{story.title}</a>
          </li>
        ))}
      </ul>
    </div>
  );
}
 
export default App;
```

```tsx
// test suit
import React from 'react';
import axios from 'axios';
import { render, screen, act } from '@testing-library/react';
import '@testing-library/jest-dom'
import userEvent from '@testing-library/user-event';


jest.mock('axios');
const mockedAxios = axios as jest.Mocked<typeof axios>

import Story from '../../src/components/Story'


describe('Story', () => {
  test('fetches stories from an API and displays them', async () => {
    const stories = [
      { objectID: '1', title: 'Hello' },
      { objectID: '2', title: 'React' }
    ]

    const promise = Promise.resolve({ data: { hits: stories } });

    mockedAxios.get.mockImplementationOnce(() => promise);

    render(<Story />)

    // await userEvent.click(screen.getByRole('button'));

    act(() => userEvent.click(screen.getByRole('button')));

    await act(() => promise);

    const items = await screen.findAllByRole('listitem')

    expect(items).toHaveLength(2)
  })

  test('fetches stories from an API and fails', async () => {
    mockedAxios.get.mockImplementationOnce(() =>
      Promise.reject(new Error())
    );

    render(<Story />);

    // await userEvent.click(screen.getByRole('button'));

    act(() => userEvent.click(screen.getByRole('button')));

    const message = await screen.findByText(/Something went wrong/);

    expect(message).toBeInTheDocument();
  });
})
```



因为是在typescript中，注意这里：

```typescript
const mockedAxios = axios as jest.Mocked<typeof axios>
```



### Tips

`screen.debug()` 在渲染组件后查看HTML输出：

```tsx
//App.tsx
import React from 'react';
 
const title = 'Hello React';
 
function App() {
  return <div>{title}</div>;
}
 
export default App;
```

```tsx
//app.test.tsx
import React from 'react';
import { render, screen } from '@testing-library/react';
 
import App from './App';
 
describe('App', () => {
  test('renders App component', () => {
    render(<App />);
 
    screen.debug();
  });
});
```

```html
/*screen.debug的输出*/
<body>
  <div>
    <div>
      Hello React
    </div>
  </div>
</body>
```



## 总结

Jest: 测试框架

- Jest在Mock这块有很重的能力，而Mock能力就是为集成测试服务的。

Testing Library: DOM Testing Library



Jest + Testing Library: 避免直接测试组件细节，模拟用户行为

- UI测试：可能是一个组件、多个组件，这是一种集成测试
- Jest不是必需的：任何提供DOM nodes都可以，无论模拟还是实际浏览器



UI测试

- 在不方便生成图形用户界面时，才考虑退而求其次的SNAPSHOT TESTING、DOM TESTING，毕竟真实的图形界面，更直观且更有保障。

- snapshot是一种轻量的方式，是可以考虑在项目中实践一下的。它的缺点，仅描述了这块UI的起点，而没有描述这块UI的意图，这也是轻量级的代价。
- Testing Library虽然有许多工具帮忙，但在实践的时候体验不是很好，理由：
  - 有的样式在单独的文件，比如控制元素显示，但因为jest使用jest-transform-stub，导致出来的样式是丢失的；
  - 在使用antd这样第三方库的时候，虽然视觉效果和纯html实现的一致，但元素使用、元素role等存在差异，导致使用查询、触发事件等api时很麻烦；
  - ...