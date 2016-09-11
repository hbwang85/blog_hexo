---
  title: React Native 初探
  category: React Native
---

## 写在最前
这篇文章是一次session的总结， 请结合[配套PPT](https://pan.baidu.com/s/1bpMbeij)，否则有些内容可能无从找起。
## React Native的起源
早在2011年，Facebook一位员工 `Jordan Walke` 因不满市面上的JavaScript MVC架构，决定和团队一起重新开发一套，于是React就诞生了。

<!--more-->

2012年React被用在了Instagram项目。Facebook内部团队认为其非常优秀，并在2013年将其开源。

由于React的设计思想及其独特，性能出众，越来越多的人开始关注和使用。随着React滚雪球似的发展，Facebook团队意识到的它的强大，认为可以“Learn once, write anywhere”, 并提出了将React用于mobile native开发的框架，这就是**React Native**.

React Native 到底有多火呢？我们可以通过`Github star`的数目窥见一斑。并且几乎每天都有pull request 被merge。

那目前React Native 都有哪些应用已经使用了呢？从图我们可以看出目前有相当多的应用已经开始使用这一优秀的架构。需要说明的是由于React Native 尚处于起步阶段，多数App处于尝试阶段，据我所知，目前除了Facebook的三款应用使用纯React Native开发，其他的App大多只是部分功能模块使用React Native开发。

##React Native的优势和不足
由于React Native 处于Hybrid和Native之间，要评价React Native，我们需要将其分别于Hybrid 和 Native 进行比较。我们首先说一下它的优势。
### React Native 优势
* RN和Native相比，它可以**Learn once， write anywhere**，只要掌握了React，开发者就可以分别进行Web开发以及mobile 开发。
* **RN支持跨平台开发**，RN工程中可以复用大量的逻辑处理和UI控件，只需要对个别不可复用的平台针对性强的UI控件做特别处理就可以实现一个工程既可以运行在iPhone设备上也可以运行在Android设备上。
* **支持热更新（Hot Code Push）**。相对iOS不太成熟的热更新，这无疑是一把利剑。App不再需要漫长的App Store审核。
* RN和Hybrid相比，由于**RN完全使用Native的UI控件进行渲染**，因此RN可以实现Native的各种动画效果，而且性能也更接近于Native。

### React Native 不足
* 首先我想说的是RN的学习曲线。由于我是纯Native开发，不懂Web开发。刚开始学习时感觉要学很多的东西，JavaScript，React， Redux， CSS等。因此个人认为开始时**学习曲线是比较陡峭的**。当然如果懂前端开发，相信这些都是不在话下的。  
* **API不稳定**。RN现在的版本为0.30. 并且每1~2周都会有一个release。这给开发者带来的痛苦不言而喻。
* **文档不完善**。官网上资源非常有限，对API的介绍也都非常的浅显。
* **对Android的支持滞后**。React Native从去年12月才开始支持Android，相对晚一些。

## 工程讲解
在开始之前，我们可以先看一下RN的性能。这个视频录制的是RN官方的一个Demo。我们重点看“ListView”部分。因为这个控件是最常用的，也是最容易引起重用等性能问题的。从视频中我们可以看到性能还是非常优秀的。  
### 工程文件结构  
首先我们来说一下工程的文件结构。从上至下依次有:  

* android文件夹： 这个是自动生成的Android原生工程。
* ios文件夹： 这个是自动生成的iOS原生工程。需要注意的是这两个原生工程我们基本上是不需要做任何修改的。那么它们存在的意义呢？提供App的入口。
* js文件夹：这个是自己定义的文件夹。一般为了分类清晰，我们可以定义Common、App等用来归类我们创建的控件（Component）。
* node_modules: 这个是工程依赖的所有node模块，像react模块，react-native模块都可以在这里找到。
* index.android.js: 这个文件是RN工程Android版入口文件
* index.ios.js: 这个文件的RN工程iOS版入口文件。这两个文件的主要作用就是创建RN的rootComponent，并且将其和自动生成的原生工程绑定。我们通过RN创建的控件都是通过rootComponent实现一级级调用的。
* package.json： 这个是node 模块的版本管理文件

### 核心代码
我为这次的session做了一个简单的demo，demo基于`ES6`，页面分两级：第一级是一个`listView`，按页请求Github的user API并刷新UI，第二级是一个详情页面，展示由上一级传递过来的数据。第一级页面我们主要练习控件的主要生命周期以及`state`的使用，第二级页面我们主要练习`property`的使用。首先我们看一下这个Demo的视频以有一个初步的认识。

#### index.ios.js
* 我们刚提到，这个是入口文件，在这个文件里我们需要定义**rootComponent**。首先要说的是react模块以及react-native模块的引用。在我开始学习RN时完全没有概念，到底什么地方应该引用哪个模块，网上sample很多时候又不统一。我们可以从react-native模块的源码入手。从源码中我们可以看到像我们常用的控件Text、View、Navigator等以及StyleSheet等都是在这里的。所以正确的引用应该是**常用的控件引用“react-native”模块**；  

* 关于引用其他自己创建的控件，由于目前生成的导出控件语法   
  `export default XX`
	所以引用时为  
	`var com = require(‘./js/xx’).default`
  
	当然，如果你采用之前的 `export module = xx` 就不在需要加 `default`了
* 绑定rootComponent和原生工程

#### AppDelegate.m
这个文件是iOS原生工程的入口文件。这里需要注意：  

* **jsCodeLocation**： 我们写的JS部分最终生成的路径就是这个url。想具体了解可以从浏览器打开这个url看看具体是什么东东。
* **RCTRootView**，这个是rootView，它会承载我们所有的UI控件。通过源代码可以看出**它继承自UIView**。
* **RN工程调用顺序**：通过此文件请求JS bundle， 然后将其rootComponent首先加载到RCTRootView，然后通过rootComponent实现其他各个控件的调用执行。

#### SearchResults.js
这个控件主要实现listview分页展示Github user 信息。首先我们需要了解React的两个概念： `State`和`Property`。

* **State**：其实就是一个**状态机**。通过改变State去出发控件的render方法去重新渲染UI。
* **Property**： 不可变，主要通过值传递方式获得值。

* 这里我们可以看到我们首先在初始化方法中初始化`State`。然后在将要加载控件时去请求网络获取数据。需要注意的是`componentWillMount`在控件的生命周期中只出现一次。另外，可以看到`_fetchData`方法 是以`_`开始，这其实是一个私有方法的约定俗称的定义方式。
* 在“_fetchData”方法中我们看到，当获取到网络数据后，我们改变State的值，以触发UI重新渲染。
* ”_selectCell”方法处理cell的点击操作，可以看到，在方法中我们实现了 “push”操作，并且将下一级所需要的值以“property”的方式传递。
* **render** 这个方法是生命周期中最重要的方法，也是一个控件不可或缺的方法。
* ListView属性内调用控件方法需要`bind(this)`,否则会提示错误`找不到该方法`，也可以通过箭函数实现bind  

#### DetailPage.js  

```
static propTypes = {
  user: PropTypes.object.isRequired,
}
static defaultProps = {
	user: {
		id: 'defaultID',
		score: 5,
	}
}
```
上述代码我们定义了`property`的条件：是类型为`Object`并且必须传入的。第二段我们定义的`property`的初始值，如果`property`值没有传入，就使用默认值。

### IDE
适用于RN的IDE有Facebook官方的`Atom + Nuclide`, `WebStorm`, `Sublime`等。这里再介绍一款IDE： [Deco](https://www.decosoftware.com/)
Deco 支持：   

* **创建新的控件** Deco可以创建控件并直接插入所需的代码；
* **拖拽插入控件** Deco支持直接将控件拖拽到相应代码位置，自动生成对应代码；
* **可视化调整属性** 拖拽的控件在IDE右侧的`Property`会罗列出多有属性，可以通过调整这里的属性调节控件；
* **选择simulator** 可以直接选择要运行的模拟器进行运行；

当然，由于Deco尚属于初期，还存在一些不足。如：  
  
* 目前只支持iOS；
* 支持拖拽的控件有限，Demo中的`ListView`目前就不支持；
* 字体不可调整， 非常小，很多提了这个issue，期望在不久的版本中可以改善。

### 样式
在上面介绍的Deco中，我们通过直接拖拽控件，然后控件的所有属性都自动生成并添加到代码中。实际上官方推荐将所有的样式集中到一起，如下代码所示：  

```
const styles = StyleSheet.create({
  container: {
    flex: 1,
    justifyContent: 'center',
    alignItems: 'center',
    backgroundColor: '#F5FCFF',
  },
});
```
RN使用类似CSS样式。对于控件的布局主要通过 `flexbox`， `justifyContent`和 `alignItems`三个属性进行控制。
### 更多
[代码在这里](https://github.com/hbwang85/RNWorkshop)

