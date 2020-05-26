### 创建项目([<u>git地址</u>](https://github.com/NiceJourney/learn-redux/tree/develop))

```bash
create-react-app <projectName>
```



### 文件夹内容修改

删除 `src` 目录下除 `index.js` 之外的所有文件, 并且删除 `index.js` 中不用的代码, 引入`TodoList` 组件.

```js
import React from "react";
import ReactDOM from "react-dom";

import TodoList from "./TodoList";

ReactDOM.render(<TodoList />, document.getElementById("root"));
```



### 文件夹结构如下: 

> **.**
> **├── README.md**
> **├── package.json**
> **├── public**
> **├── src**
> **│   ├── TodoList.js**							          // TodoList 组件的逻辑部分
> **│   ├── TodoListUI.js**						         // TodoList 组件的UI部分
> **│   ├── index.js**									        // 入口文件
> **│   └── store**											      // store文件夹
> **│       ├── actionCreators.js**				    // action逻辑
> **│       ├── actionTypes.js**						  // action类型
> **│       ├── index.js**									    // store的入口配置文件
> **│       └── reducer.js**									// reducer配置文件
> **└── yarn.lock**



### 安装依赖

```bash
# 根据实际情况安装依赖
yarn add axios antd redux redux-thunk
```



### 创建 `store`

`store` 文件夹下新建`index.js`文件, 内容为: 

```js
import { createStore, applyMiddleware, compose } from "redux";
import reducer from "./reducer";
import thunk from "redux-thunk";

// __REDUX_DEVTOOLS_EXTENSION__ 指浏览器redux_devtools扩展
const composeEnhancers = window.__REDUX_DEVTOOLS_EXTENSION_COMPOSE__
  ? window.__REDUX_DEVTOOLS_EXTENSION_COMPOSE__({})
  : compose;

// applyMiddleware ==> 中间件
const enhancer = composeEnhancers(applyMiddleware(thunk));

// createStore() 接受两个参数
const store = createStore(reducer, enhancer);

export default store;

```



### 创建 `reducer.js`

```js
import {
  CHANGE_INPUT_VALUE,
  ADD_ITEM,
  DELETE_ITEM,
  GET_LIST,
} from "./actionTypes";  // 导入actionType
// 定义默认数据
export const defaultState = {
  inputValue: "Writing something",
  list: [],
};

export default (state = defaultState, action) => {
  // 通过switch进行判断
  switch (action.type) {
    case CHANGE_INPUT_VALUE: {
      // state值只能传递，不能使用
      let newState = JSON.parse(JSON.stringify(state));  // 深拷贝
      newState.inputValue = action.value;
      return newState;
    }
    case ADD_ITEM: {
      let newState = JSON.parse(JSON.stringify(state));
      newState.list.push(newState.inputValue);
      newState.inputValue = "";
      return newState;
    }
    case DELETE_ITEM: {
      let newState = JSON.parse(JSON.stringify(state));
      newState.list.splice(action.index, 1);
      return newState;
    }
    case GET_LIST: {
      let newState = JSON.parse(JSON.stringify(state));
      newState.list = action.data.list;
      return newState;
    }
    default:
      return state;
  }
};

```



### 创建 `actionTypes.js` ==> 统一导出 `action.type`, 方便管理

```js
export const CHANGE_INPUT_VALUE = "changeInputValue";
export const ADD_ITEM = "addItem";
export const DELETE_ITEM = "deleteItem";
export const GET_LIST = "getList";

```



### 创建 `actionCreators.js` ==> 统一存放 `action`

```js
import axios from "axios";

import {
  CHANGE_INPUT_VALUE,
  ADD_ITEM,
  DELETE_ITEM,
  GET_LIST,
} from "./actionTypes";

// 每个action必须有一个"type"属性
export const changeInputValueAction = (value) => ({
  type: CHANGE_INPUT_VALUE,
  value,
});

// 不接受参数
export const addItemAction = () => ({ type: ADD_ITEM });

// 接受参数
export const deleteItemAction = (index) => ({ type: DELETE_ITEM, index });

export const getListAction = (data) => ({
  type: GET_LIST,
  data,
});

// return一个网络请求, 并传入dispatch
export const getRemoteList = () => {
  return (dispatch) => {
    axios
      .get("http://rap2.taobao.org:38080/app/mock/data/1561907")
      .then((res) => {
        const data = res.data;
        const action = getListAction(data);
        dispatch(action);
      });
  };
};

```



### 创建 `TodoList.js` , `TodoListUI.js`文件

#### `TodoList.js`: 

```js
import React from "react";

import store from "./store/index";

import {
  changeInputValueAction,
  addItemAction,
  deleteItemAction,
  getRemoteList,
} from "./store/actionCreators";

import TodoListUI from "./TodoListUI";

class TodoList extends React.Component {
  constructor(props) {
    super(props);

    this.state = store.getState();  // 获取store中的值
    store.subscribe(this.storeChange);  // 通过subscribe来获取数据更新
  }

  componentDidMount = () => {
    const action = getRemoteList();  // 不接受参数的action
    store.dispatch(action);  // store.dispatch推送action通过reducer更新store
  };

  changeInputValue = (e) => {
    const action = changeInputValueAction(e.target.value);  // 接受参数的action
    store.dispatch(action);
  };

  onSubmit = () => {
    const action = addItemAction();
    store.dispatch(action);
  };

  deleteItem = (index) => {
    const action = deleteItemAction(index);
    store.dispatch(action);
  };

  storeChange = () => {
    // 当store更新之后, 调用this.setState()后重新render页面, 获取最新值
    this.setState(store.getState());
  };

  render() {
    return (
      // 使用函数式组件让网页有更好的性能
      <TodoListUI
        inputValue={this.state.inputValue}
        list={this.state.list}
        changeInputValue={this.changeInputValue}
        onSubmit={this.onSubmit}
        deleteItem={this.deleteItem}
      />
    );
  }
}

export default TodoList;

```



#### `TodoListUI.js`: 

```js
import React from "react";
import "antd/dist/antd.css";
import { Input, Button, List } from "antd";

const TodoListUI = (props) => {
  return (
    <div>
      <div>
        <Input
          placeholder={props.inputValue}
          value={props.inputValue}
          style={{ width: "200px", marginRight: "10px" }}
          onChange={props.changeInputValue}
        />
        <Button type="primary" onClick={props.onSubmit}>
          submit
        </Button>
      </div>
      <List
        bordered
        dataSource={props.list}
        renderItem={(item, index) => (
          <List.Item onClick={() => props.deleteItem(index)}>{item}</List.Item>
        )}
      />
    </div>
  );
};

export default TodoListUI;

```

