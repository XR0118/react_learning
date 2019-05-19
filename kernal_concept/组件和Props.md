# 组件 & Props

**组件的核心作用**：允许你将 UI 拆分为独立可复用的代码片段，并对每个片段进行独立构思。

## 函数组件与 class 组件

函数组件例子：

```js
function Welcome(props) {
  return <h1>Hello, {props.name}</h1>;
}
```

函数组件定义：接收唯一带有数据的 “props”（代表属性）对象与并返回一个 React 元素。

ES6 class 组件:

```js
class Welcome extends React.Component {
  render() {
    return <h1>Hello, {this.props.name}</h1>;
  }
}
```

为了保持示例简单明了，**本章将使用函数组件**。

## 渲染组件

React 元素也可以是用户自定义的组件：

```js
const element = <Welcome name="Sara" />;
```

当 React 元素为用户自定义组件时，它会将 JSX 所接收的属性（attributes）转换为单个对象传递给组件，这个对象被称之为 “props”。
简单直观来说，就是组件的属性会以 `props` 变量传入组件，**类似于父节点的属性传入子节点**。例子：

```js
function Welcome(props) {
  return <h1>Hello, {props.name}</h1>;
}

const element = <Welcome name="Sara" />;
ReactDOM.render(
  element,
  document.getElementById('root')
);
```

**tips**: 组件名称必须以大写字母开头。React 会将以小写字母开头的组件视为原生 DOM 标签。例如，`<div />` 代表 HTML 的 div 标签，而 `<Welcome />` 则代表一个组件，并且需在作用域内使用 Welcome。

## 组合组件

创建一个可以多次渲染 Welcome 组件的 App 组件：

```js
function Welcome(props) {
  return <h1>Hello, {props.name}</h1>;
}

function App() {
  return (
    <div>
      <Welcome name="Sara" />
      <Welcome name="Cahal" />
      <Welcome name="Edite" />
    </div>
  );
}

ReactDOM.render(
  <App />,
  document.getElementById('root')
);
```

通常来说，每个新的 React 应用程序的顶层组件都是 App 组件。但是，如果你将 React 集成到现有的应用程序中，你可能需要使用像 Button 这样的小组件，并自下而上地将这类组件逐步应用到视图层的每一处。

## 提取组件

将复杂组件进行拆分，从而获得细粒度更细的小组件，完成组件的复用。

参考例子：

```js
// 原先的 Comment 组件
function Comment(props) {
  return (
    <div className="Comment">
      <div className="UserInfo">
        <img className="Avatar"
          src={props.author.avatarUrl}
          alt={props.author.name}
        />
        <div className="UserInfo-name">
          {props.author.name}
        </div>
      </div>
      <div className="Comment-text">
        {props.text}
      </div>
      <div className="Comment-date">
        {formatDate(props.date)}
      </div>
    </div>
  );
}

// 提取出 Avatar 组件
function Avatar(props) {
    return (
        <img className="Avatar"
          src={props.user.avatarUrl}
          alt={props.user.name}
        />
    );
}
// 再提取 UserInfo 组件
function UserInfo(props) {
    return (
        <div className="UserInfo">
            <Avatar user={props.user} />
            <div className="UserInfo-name">
                {props.user.name}
            </div>
        </div>
    )
}
// Comment 组件变为
fuction Comment(props) {
    return (
        <div className="Comment">
            <UserInfo user={props.author}/>
            <div className="Comment-text">
                {props.text}
            </div>
            <div className="Comment-date">
                {formatDate(props.date)}
            </div>
        </div>
    );
}
```

根据经验来看，如果 UI 中有一部分被多次使用（`Button`，`Panel`，`Avatar`），或者组件本身就足够复杂（`App`，`FeedStory`，`Comment`），那么它就是一个可复用组件的候选项。

## Props 的只读性

组件无论是使用函数声明还是通过 class 声明，都**决不能**修改自身的 props。
**所有 React 组件都必须像纯函数一样保护它们的 props 不被更改。**
