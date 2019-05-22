# State & 生命周期

简单的始终组件例子:

```js
function Clock(props) {
    return (
    <div>
        <h1>this is a clock</h1>
        <h2> It is {props.date.toLocaleTimeString()} </h2>
    </div>
    );
}
function tick() {
    ReactDOM.render(
        <Clock date={new Date()} />,
        document.getElementById('root')
    )
}

setInterval(tick, 1000);
```

在 `Clock` 组件中使用 state 属性，让其自动更新。
State 与 props 类似，但是 state 是私有的，并且完全受控于当前组件。

## 函数组件转换为 Class 组件

转换步骤：

1. 创建一个同名的 ES6 class，并且继承于 React.Component。

2. 添加一个空的 render() 方法。

3. 将函数体移动到 render() 方法之中。

4. 在 render() 方法中使用 this.props 替换 props。

5. 删除剩余的空函数声明。

```js
class Clock extends React.Compment {
    render(
        return (
            <div>
                <h1>Clock</h1>
                <h2>it is {this.props.date.toLocaleTimeString()}</h2>
            </div>
        )
    )
}
```

## 向 class 组件中添加局部的 state

1. 通过 class 的构造函数来添加 state

```js
class Clock extends React.Compment {
    constructor(props) {
        super(props); // 将 props 传递到父类中
        this.state = {date: new Date()};
    }

    render(
        return (
            <div>
                <h1>Clock</h1>
                <h2>it is {this.state.date.toLocaleTimeString()}</h2>
            </div>
        )
    )
}

ReactDOM.render(
  <Clock />,
  document.getElementById('root')
);
```

## 在 Class 中添加生命周期方法

在组件生成时，申请并占用资源；在组件销毁时，释放其占用的资源。

组件生成的过程是 `Mount`, 销毁则是 `Unmount`

在 Clock 中加入生命周期函数，达到生成时加入定时器，销毁时清除定时器的目的

```js
class Clock extends React.Component {
    constructor(props) {
        super(props);
        this.state = {date: new Date()};
    }

    tick() {
        this.setState({
            date: new Date(),
        });
    }

    componentDidMount () {
        this.t1 = setInterval(
        () => this.tick(),
        1000
        );
    }

    componentWillUnmount () {
        clearInterval(this.t1);
    }

    render () {
        return (
            <div>
                <h1>Clock</h1>
                <h2>it is {this.state.date.toLocaleTimeString()}</h2>
            </div>
        )
    }
}

ReactDOM.render(
<Clock />,
document.getElementById('root')
);
```

注意：

1. 生命周期函数的拼写问题

2. 箭头函数和普通函数在 class 中的区别:
箭头函数和普通函数中的 this 指向的对象不同，箭头函数中指向 Clock;普通函数中指向 Window

## 正确使用 State

1. 使用 setState() 修改 state 而不是直接修改

2. 异步更新，出于性能考虑，React 可能会把多个 setState() 调用合并成一个调用。

3. state 的更新会被合并

## 数据是向下流动的

不管是父组件或是子组件都无法知道某个组件是有状态的还是无状态的，并且它们也并不关心它是函数组件还是 class 组件。

这通常会被叫做“自上而下”或是“单向”的数据流。任何的 state 总是所属于特定的组件，而且从该 state 派生的任何数据或 UI 只能影响树中“低于”它们的组件。


