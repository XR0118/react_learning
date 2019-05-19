# JSX 简介

## 在 JSX 中嵌入表达式

```js
const name = 'Josh Perez';
const element = <h1>Hello, {name}</h1>;

ReactDOM.render(
  element,
  document.getElementById('root')
);
```

JSX 语法中可以再大括号中放置任意有效的 JS 表达式，例子：

```js
function formatName(user) {
    return user.firstName + ' ' + user.lastName
}

const user = {
    firstName: 'Jhon',
    lastName: 'Perez',
};

const element = <h1>Hello, {formatName(user)}</h1>;

ReactDOM.render(
  element,
  document.getElementById('root')
);
```

## JSX 也可以作为表达式

可以在 if 语句和 for 循环的代码块中使用 JSX，将 JSX 赋值给变量，把 JSX 当作参数传入，以及从函数中返回 JSX：

```js
function getGreeting(user) {
  if (user) {
    return <h1>Hello, {formatName(user)}!</h1>;
  }
  return <h1>Hello, Stranger.</h1>;
}
```

## JSX 特定属性的使用

```js
const element = <div tabIndex="0"></div>;
const element = <img src={user.avatarUrl}></img>;
```

在属性中嵌入 JavaScript 表达式时，不要在大括号外面加上引号。你应该仅使用引号（对于字符串值）或大括号（对于表达式）中的一个，**对于同一属性不能同时使用这两种符号**。

**tip**:React DOM 使用 camelCase（小驼峰命名）来定义属性的名称，而不使用 HTML 属性名称的命名约定. 例如，JSX 里的 class 变成了 className，而 tabindex 则变为 tabIndex。

## 指定子元素

假如一个标签里面没有内容，你可以使用 `/>` 来闭合标签，就像 XML 语法一样:

```js
const element = <img src={user.avatarUrl} />;
// 包含多个子元素
const element = (
  <div>
    <h1>Hello!</h1>
    <h2>Good to see you here.</h2>
  </div>
);
```

## JSX 表示对象

Babel 会把 JSX 转译成一个名为 `React.createElement()` 函数调用。

```js
const element = (
  <h1 className="greeting">
    Hello, world!
  </h1>
);
// 等价于
const element = React.createElement(
  'h1',
  {className: 'greeting'},
  'Hello, world!'
);
// 实际创建的对象是
// 注意：这是简化过的结构
const element = {
  type: 'h1',
  props: {
    className: 'greeting',
    children: 'Hello, world!'
  }
};
```

这些对象被称为 “React 元素”。它们描述了你希望在屏幕上看到的内容。React 通过读取这些对象，然后使用它们来构建 DOM 以及保持随时更新。
