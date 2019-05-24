---
title: React初学(2019-4-2)
---

# 主要概念
#### 一、 列表&keys
1. 在不用打包的项目中如果要展示一个列表，这个列表中的数据从一个接口中获取，那就要用到`for`循环了。但是`react`中没有类似`vue`的`v-for`，所以对于这种需求，在 `react`中，一般采用数组的`map`方法，如以下例子所示

    ```js
    const numbers = [1, 2, 3, 4, 5];
    const listItems = numbers.map((number) =>
      <li>{number}</li>
    );
    ```

2. 需要注意的一点是，react要求列表的每一项都有一个唯一的`key`，这样可以准确的对每一项进行修改，如下：

    ```js
    const todoItems = todos.map((todo, index) =>
      <li key={index}>
        {todo.text}
      </li>
    );
    ```
    但是在官方文档中提出，如果组件的顺序发生变换就不推荐使用`map`中的`index`作为`key`，因为这可能对性能造成负面影响，并且可能导致组件状态出现问题

3. 还有一点需要注意，就是`key`的添加位置，不能单纯加载类似`<li>`标签的上面，而是要结合数组的`map`，如下：

    ```js
    function ListItem(props) {
      // 在这里不需要加key
      return <li>{props.value}</li>;
    }
    
    function NumberList(props) {
      const numbers = props.numbers;
      const listItems = numbers.map((number) =>
        // 这里用到数组的map，所以加载这里
        <ListItem key={number.toString()}
                  value={number} />
    
      );
      return (
        <ul>
          {listItems}
        </ul>
      );
    }
    
    const numbers = [1, 2, 3, 4, 5];
    ReactDOM.render(
      <NumberList numbers={numbers} />,
      document.getElementById('root')
    );   
    ```
4. 对于key的唯一性是相对于兄弟节点之间的，但是全局之下，对于不同的数组，是可以用相同的`key`值
5. 对于写法的优化：`map`可以直接嵌套在`jsx`中

    ```js
    function NumberList(props) {
      const numbers = props.numbers;
      const listItems = numbers.map((number) =>
        <ListItem key={number.toString()}
                  value={number} />
    
      );
      return (
        <ul>
          {listItems} //这里没有嵌套
        </ul>
      );
    }
    
    function NumberList(props) {
      const numbers = props.numbers;
      return (
        <ul>
          {numbers.map((number) => //嵌套
            <ListItem key={number.toString()}
                      value={number} />
    
          )}
        </ul>
      );
    }
    ```

#### 二、表单
1. 在官方文档中，关于表单介绍了几种形式：`<input>`,`<textarea>`, `<select>`，这些组件的值都是依据用户输入，因此用户输入就是唯一的数据源，因此这个唯一的数据源是控制这些组件的变化，这些组件也就是受控组件
2. 以上组件都有`value`属性，这个属性就是用户输入的值。值得一提的是在`<select>`中，只需要从根节点获取`value`即可，如下：
    
    ```js
    <select value={this.state.value} onChange={this.handleChange}>
        <option value="grapefruit">Grapefruit</option>
        <option value="lime">Lime</option>
        <option value="coconut">Coconut</option>
        <option value="mango">Mango</option>
    </select>
    ```
3. 如何控制多个输入，在`es6`中提出一种新语法，它可以跟新给定的状态的值：
    ```js
    this.setState({
      [name]: value
    });
    ```

#### 三、状态提升
1. 他的理念就是：如果两个组件依据同一个状态值需要同步的更新，那么这个状态值就需要从这两个组件提升至他们最邻近的父组件中。

#### 四、组装
1. 因为涉及的组件的复用性，所以会讲到组装，何为组装，就是特定场景的组件引用一种通用的组件。这种通用的组件可以被用在多个地方，他们可以被当做子组件，而引用这些组件的就是这些组件的父组件，父组件负责向子组件传入特定的值，渲染出特定的组件。
2. 这种传值通过`props.children`以及其他的想要传递的其他名称，可以传递普通的值，也可以传入你想要的任何对象(`function`或`react elements`)
    ```js
    function SplitPane(props) {
    return (
        <div className="SplitPane">
          <div className="SplitPane-left">
            {props.left}
          </div>
          <div className="SplitPane-right">
            {props.right}
          </div>
        </div>
      );
    }

    function App() {
      return (
        <SplitPane
          left={
            <Contacts /> //这里传递的就是其他的组件
          }
          right={
            <Chat />
          } />
      );
    }
    ```

    ```js
    function Dialog(props) { //这是子组件
      return (
        <FancyBorder color="blue">
          {props.children}
        </FancyBorder>
      );
    }

    class SignUpDialog extends React.Component { //这是父组件
      constructor(props) {
        super(props);
        this.state = {login: ''};
      }

      render() {
        return (
          <Dialog >
            <input value={this.state.login}
                   onChange={this.handleChange} />
            <button onClick={this.handleSignUp}>
              Sign Me Up!
            </button> //这里面的所有内容都对应props.children，父组件向子组件传值
          </Dialog>
        );
      }

    }
    ```
3. 在何种场景去运用组装呢？官方文档中指出，如果想复用组件的功能而与`UI`无关，这个时候就可以将这个功能抽离出来成为一个独立的组件，这样其他组件可以`import`这个组件，而不需要对他进行任何扩展

#### 五、react理念——组件的拆分与状态的选取
1. 第一步：把`UI`划分成层级组件。划分的原则基于单一职责原则，而且这种拆分和设计者在`PhotoShop`中的图层是类似的
2. 第二步：用`React`创建一个静态的版本。在这一步，完全不需要用到任何的`state`，可以从层级关系的上到下，也可以下到上。这些组件只有一个方法就是`render`。在这里需要搞清楚`props`和`state`的区别，前者是从父组件获取到的状态值，后者是此组件自己的状态值
3. 第三部：定义`UI`状态最小的单是完整的表示，也就是明确带有状态的组件。这里需要保证不要重复，对于`state`的区分把握三个原则：
  * 如果他是从父组件传来的，可能不是`state`
  * 如果他不会随着时间变化，可能不是`state`
  * 如果他能根据组件中的其他`state`或`props`计算得来，可能不是`state`
4. 第四步：明确`state`应该位于哪个组件，首先需要对`state`做出如下理解：
  * 明确基于这个状态渲染出来的每一个组件
  * 找到这个状态的共同拥有者以及他们的高一级的需要用到这个状态的组件
  * 共同拥有状态的组件还是高一级的组件都需要用到这个状态
  * 如果你没有找到可以拥有这个状态的组件，可以创建一个仅仅用来保存状态的组件放在共同拥有某状态的组件的上一级
5. 第五步：添加反向数据流。这一步的目的是为了能让子组件去改变父组件的状态，因为有些时候状态的改变是从子组件触发的。这个时候需要父组件向子组件传递回调函数了，这个函数在父组件中定义，触发在子组件，这样就可以达到目的