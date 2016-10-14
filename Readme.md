# React要点入门学习总结

## 一.JSX简介

JSX即JavaScript XML,一种在React组件内部构建标签的类XML语法。
在不使用JSX的情况下，React程序中创建DOM是这样的：

    //v0.11
    React.DOM.h1({className: 'title'}, 'Title');
    //v0.12
    React.createElement('h1', {className: 'title'}, 'Title');

如果使用JSX的方式创建节点为：

    <h1 className="title">Title</h1>

JSX的特征：
1. JSX是一种句法变换，每一个JSX节点都对应着一个JavaScript函数
2. JSX即不提供也不需要运行时库
3. JSX并没有改变或者添加JavaScript的语义，它只是简单的函数调用。


### 代码一：

    <!DOCTYPE html>
    <html>
      <head>
        <meta charset="utf-8">
      </head>
      <body>
        <div id="example"></div>
        <script src="../build/react.js"></script>
        <script src="../build/react-dom.js"></script>
        <script src="https://cdnjs.cloudflare.com/ajax/libs/babel-core/5.8.24/browser.min.js"></script>
        <script type="text/babel">
          ReactDOM.render(
            <h1>Hello,react!</h1>,
            document.getElementById('example')
          );
        </script>
      </body>
    </html>

要点一：在整个Html文件中，只有定义了一个id为example的div标签，当程序运行时，JavaScript代码执行，
React会创建新的DOM节点，也就是<h1>Hello,react!</h1>。
ReactDOM.render(newDom,parentDom);这个函数是用来对视图进行渲染新的节点，函数的参数主要有两个，
一个是新的节点，另一个是新节点要放在哪个父节点中。

要点二：<script> 标签的 type 属性为 "text/babel" 。这是因为 React 独有的 JSX 语法，跟 
JavaScript 不兼容。凡是使用 JSX 的地方，都要加上 type="text/babel"

要点三：ReactDOM.render()方法中的第一个参数，也就是新的节点，只能有一个顶层的标签，然后里面嵌套多个子节点，
例如下面的写法就是不可行的：

    //Error
    ReactDOM.render(
      <p>Error</P>
      <h1>Hello,react!</h1>,
      document.getElementById('example')
    );


## 二.动态数值

### 代码二：

    <!DOCTYPE html>
    <html>
      <head>
        <meta charset="utf-8">
      </head>
      <body>
        <div id="example"></div>
        <script src="../build/react.js"></script>
        <script src="../build/react-dom.js"></script>
        <script src="https://cdnjs.cloudflare.com/ajax/libs/babel-core/5.8.24/browser.min.js"></script>
        <script type="text/babel">
          var names = ['lgy','Kb'];
          ReactDOM.render(
            <div>
            {
              names.map((name) => {
                return <div>Hello,{name} !</div>
              })
            }
            </div>,
            document.getElementById('example')
          );
        </script>
      </body>
    </html>

要点：通过在JavaScript中定义变量，可嵌入JSX中使用动态变量，使用的语法为：<div>Hello,{name} !</div>

### 代码三：

    <!DOCTYPE html>
    <html>
      <head>
        <meta charset="utf-8">
      </head>
      <body>
        <div id="example"></div>
        <script src="../build/react.js"></script>
        <script src="../build/react-dom.js"></script>
        <script src="https://cdnjs.cloudflare.com/ajax/libs/babel-core/5.8.24/browser.min.js"></script>
        <script type="text/babel">
          var arr = [
            <h1>Hello react</h1>,
            <h2>Hello Node</h2>
          ];
          ReactDOM.render(
            <div>{arr}</div>,
            document.getElementById('example')
          );
        </script>
      </body>
    </html>

要点：可以通过传入节点数组的方式，直接把数组给予新的节点，<div>{arr}</div>，JSX语法会根据数组的元素动态生成对应的子节点



## 三.组件构造使用

React的组件化模式是最大的亮点，组件中使用props或者state，当这两个变量改变时，相应的DOM表现也会有所改变，这主要原因是一个
组件是一个状态机，对于特定的输入，他总会返回一致的输出。

### 代码四：

    <!DOCTYPE html>
    <html>
      <head>
        <meta charset="utf-8">
      </head>
      <body>
        <div id="example"></div>
        <script src="../build/react.js"></script>
        <script src="../build/react-dom.js"></script>
        <script src="https://cdnjs.cloudflare.com/ajax/libs/babel-core/5.8.24/browser.min.js"></script>
        <script type="text/babel">
          console.log(React);
          class HelloMessage extends React.Component{
            render() {
              return <h1>Hello {this.props.name}</h1>;
            }
          }
          ReactDOM.render(
            <HelloMessage name="LGY" />,
            document.getElementById('example')
          );
        </script>
      </body>
    </html>

要点一： 构造一个组件的语法使用ES6的标准进行，通过继承React.Component,使用render() {...}进行渲染对应的标签和变量输出组件

要点二： 命名新构造的组件的第一个字母需要大写！！！

要点三： 使用构造好的组件，通过使用标签<HelloMessage name="LGY"/>，其中name是传递进去的参数，在内部用this.props.name进行
调用变量


## 四.组件的生命周期

### (1)组件的三个生命周期状态：

1.Mounting:已插入真实 DOM

2.Updating:正在被重新渲染

3.Unmounting:已移出真实 DOM

### (2)组件生命周期方法：

1.componentWillMount():改方法在完成首次渲染之前被调用。是在render方法调用前可以修改组件state的最后一次机会

2.componentDidMount():当render方法成功调用后，且DOM已经被渲染，可以在componentDidMount内部通过this.getDOMNode()方法访问到DOM节点

3.componentWillUpdate(object nextProps, object nextState):组件在收到新的props或者state进行渲染之前，这时调用该方法

4.componentDidUpdate(object prevProps, object prevState):这个方法可以让我们更新已经渲染好的DOM的机会

5.componentWillUnmount():当组件的生命结束时，这个方法就会被调用

### (3)两种特殊状态的处理函数

1.componentWillReceiveProps(object nextProps):已加载组件收到新的参数时调用

2.shouldComponentUpdate(object nextProps, object nextState):组件判断是否重新渲染时调用

## 五.数据流

### (1)Props: 

通过props可以把任意类型的数据传递给组件

    var tables = [{title: 'React'}]
    <TableList tables={tables} />

可以通过this.props.tables访问对应的属性，然绝对不能修改。一个组件绝对不可以自己修改自己的props!!!

### (2)PropsTypes: 

通过在组件中定义一个配置对象。组件初始化时，如果传递的属性和propsTypes不匹配，就会打印一个console.warn日志，
如果是可选的配置，就可以去掉isRequired。

### 代码五: 

    <!DOCTYPE html>
    <html>
    <head>
      <meta charset="utf-8">
    </head>
    <body>
      <div id="example"></div>
      <script src="../build/react.js"></script>
      <script src="../build/react-dom.js"></script>
      <script src="https://cdnjs.cloudflare.com/ajax/libs/babel-core/5.8.24/browser.min.js"></script>
      <script type="text/babel">
        class MyTitle extends React.Component{
          render() {
            return <h1>{this.props.title}</h1>;
          }
        }
        MyTitle.propTypes = {
          title: React.PropTypes.string.isRequired
        }
        //设置默认属性
        MyTitle.defaultProps = {
          title: "lgy"
        }
        var data = 'KB';
        ReactDOM.render(
          <MyTitle title={data}/>,
          document.getElementById('example')
        );
      </script>
    </body>
    </html>

要点: 示例中定义了一个组件MyTitle，其中使用propTyps对组件的属性类型进行匹配，defaultProps对组件的属性类型设置默认值。


### 代码六: 

    <!DOCTYPE html>
    <html>
      <head>
        <meta charset="utf-8">
      </head>
      <body>
        <div id="example"></div>
        <script src="../build/react.js"></script>
        <script src="../build/react-dom.js"></script>
        <script src="https://cdnjs.cloudflare.com/ajax/libs/babel-core/5.8.24/browser.min.js"></script>
        <script type="text/babel">
          class NodeList extends React.Component{
            render() {
              return (
                <ol>
                {
                  React.Children.map(this.props.children,(child) => {
                    return <li>{child}</li>;
                  })
                }
                </ol>
              );
            }
          }
          ReactDOM.render(
            <NodeList>
              <span>Hello!</span>
              <p>LGY</p>
            </NodeList>,
            document.getElementById('example')
          );
        </script>
      </body>
    </html>

要点: 通过this.props.children来获取组件的子节点

### (3)State

每个React组件都可以拥有自己的state，state与props的区别在于前者只存在于组件的内部中，this.props 
表示那些一旦定义，就不再改变的特性，而 this.state 是会随着用户互动而产生变化的特性。state可以确定
视图的状态。

### 代码七:

    <!DOCTYPE html>
    <html>
    <head>
      <meta charset="utf-8">
    </head>
    <body>
      <div id="example"></div>
      <script src="../build/react.js"></script>
      <script src="../build/react-dom.js"></script>
      <script src="https://cdnjs.cloudflare.com/ajax/libs/babel-core/5.8.24/browser.min.js"></script>
      <script type="text/babel">
        class LikeButton extends React.Component {
          constructor(props) {
            super(props);
            console.log(this);
            this.state = {liked: false};
          }
          handleClick(event) {
            console.log(this);
            this.setState({liked: !this.state.liked});
          };
          render() {
            var text = this.state.liked ? 'like' : 'haven\' t liked';
            return(
              <div>
                <input
                  type="button" 
                  value="Click to toggle." 
                  onClick={this.handleClick.bind(this)}
                />
                <p>You {text} this.</p>
              </div>
            );
          }
        }
        ReactDOM.render(
          <LikeButton />,
          document.getElementById('example')
        );
      </script>
    </body>
    </html>

要点：组件LikeButton通过constructor构造函数进行初始化状态，this.state = {liked: false} 把liked的状态设置为false,
然后通过handleClick绑定this,当用户点击按钮时，改变liked变量的状态，这时视图也会随着一起改变。

## 六.DOM操作

组件并不是真实的 DOM 节点，而是存在于内存之中的一种数据结构，叫做虚拟 DOM （virtual DOM）。只有当它
插入文档以后，才会变成真实的 DOM 。根据 React 的设计，所有的 DOM 变动，都先在虚拟 DOM 上发生，然后
再将实际发生变动的部分，反映在真实 DOM上，这种算法叫做 DOM diff ，它可以极大提高网页的性能表现。

### 代码八：

    <!DOCTYPE html>
    <html>
    <head>
      <meta charset="utf-8">
    </head>
    <body>
      <div id="example"></div>
      <script src="../build/react.js"></script>
      <script src="../build/react-dom.js"></script>
      <script src="https://cdnjs.cloudflare.com/ajax/libs/babel-core/5.8.24/browser.min.js"></script>
      <script type="text/babel">
        var MyComponent = React.createClass({
        handleClick: function() {
          if (this.myTextInput !== null) {
            this.refs.myText.focus();
          }
        },
      render: function() {
          return (
            <div>
              <input type="text" ref="myText" />
              <input
                type="button"
                value="Focus the text input"
                onClick={this.handleClick}
              />
            </div>
          );
        }
      });
      ReactDOM.render(
        <MyComponent />,
        document.getElementById('example')
      );
      </script>
    </body>
    </html>

要点：组件 MyComponent 的子节点有一个文本输入框，用于获取用户的输入。这时就必须获取真实的 DOM 节点，虚拟 DOM 是拿不到用户
输入的。为了做到这一点，文本输入框必须有一个 ref 属性，然后 this.refs.[refName] 就会返回这个真实的 DOM 节点。需要注意的
是，由于 this.refs.[refName] 属性获取的是真实 DOM ，所以必须等到虚拟 DOM 插入文档以后，才能使用这个属性，否则会报错。上
面代码中，通过为组件指定 Click 事件的回调函数，确保了只有等到真实 DOM 发生 Click 事件之后，才会读取 this.refs.[refName] 
属性。


## 七.表单

### 代码九：

    <!DOCTYPE html>
    <html>
    <head>
      <meta charset="utf-8">
    </head>
    <body>
      <div id="example"></div>
      <script src="../build/react.js"></script>
      <script src="../build/react-dom.js"></script>
      <script src="https://cdnjs.cloudflare.com/ajax/libs/babel-core/5.8.24/browser.min.js"></script>
      <script type="text/babel">
        class Input extends React.Component {
          constructor(props) {
            super(props);
            this.state = {value:'Hello!'}
          }
          handleChange(event) {
            this.setState({value:event.target.value});
          }
          render() {
            var value = this.state.value;
            return (
              <div>
                <input type="text" value={value} onChange={this.handleChange.bind(this)}/>
                <p>{value}</p>
              </div>
            );
          }
        }
        ReactDOM.render(
          <Input/>,
          document.getElementById("example")
        );
      </script>
    </body>
    </html>

要点：文本输入框的值，不能用 this.props.value 读取，而要定义一个 onChange 事件的回调函数，通过
 event.target.value 读取用户输入的值。textarea 元素、select元素、radio元素都属于这种情况




## 八.网络请求

### 代码十:

    <!DOCTYPE html>
    <html>
    <head>
      <meta charset="utf-8">
    </head>
    <body>
      <div id="example"></div>
      <script src="../build/react.js"></script>
      <script src="../build/react-dom.js"></script>
      <script src="../node_modules/jquery/dist/jquery.min.js"></script>
      <script src="https://cdnjs.cloudflare.com/ajax/libs/babel-core/5.8.24/browser.min.js"></script>
      <script type="text/babel">
        function get(source,callback) {
          var request = new Request(source);
          fetch(request)
          .then(res => res.json())
          .then(arr => callback(arr[0])) 
        }
        class UserGist extends React.Component {
          constructor(props) {
            super(props);
            this.state = {username: '',lastGistUrl: ''};
          }
          componentDidMount() {
            var usr = get(this.props.source,(usr) => {
              this.setState({
                username: usr.owner.login,
                lastGistUrl: usr.html_url
              });
            });
          }
        render() {
          var value = this.state.value;
          return (
            <div>
              {this.state.username}'s last gist is
              <a href={this.state.lastGistUrl}>here</a>.
            </div>
          );
        } 
      }
      ReactDOM.render(
        <UserGist source="https://api.github.com/users/octocat/gists" />,
        document.getElementById("example")
      );
      </script>
    </body>
    </html>

要点：该实例代码使用了Fetch API来进行数据获取，Fetch API的具体如何使用在接下来的博客文章会介绍。









