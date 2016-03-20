---
title: Android简洁架构设计
tags:
  - Android
date: 2015-11-29 21:48:03
---

> 英文出处：fernandocejas.com。

### 开始

我们知道编写高质量软件是既困难又复杂的：不仅是满足需求方面，还要健壮、可维护、可测试，并且足够灵活以适应增长和变化。这就是“代码整洁之道”的来源，并可以成为开发任何软件应用程序的良好方法。思想很简单：代码整洁之道代表构建系统的一组实践：

1.  独立于框架
2.  可测试性
3.  独立于UI
4.  独立于数据库
5.  独立于任何外部代理

![](http://jbcdn2.b0.upaiyun.com/2015/07/c169972aba197d0b1e79922aa554bfb7.png)

只限于使用图示中的4个圆圈并不是必须的，因为这只是语义描述，你还要考虑依赖规则（the Dependency Rule）：源码依赖只应该指向内圈，内圈不应该知道外圈的任何东西。

下面的相关词汇可以帮助熟悉理解这个方法：

实体（Entities）：应用的逻辑对象。
用例（Use Cases）：用例编排数据流从实体的流入和流出。也叫交互器（Interactor）。
接口适配器（Interface Adapters）：这些适配器将数据转换为用例和实体最合适的格式。表示器（Presenter）和控制器（Controller）就位于这里。
框架和驱动器（Frameworks and Drivers）：这是所有细节集中的地方，UI、工具、框架等。
为了更好更深入地理解，看下[这篇文章](http://blog.8thlight.com/uncle-bob/2012/08/13/the-clean-architecture.html)

### Android架构

架构的目标是通过将业务规则与外部世界隔离以分离关注点，这样，才可以在不依赖外部元素的情况下测试业务规则。为了达到这个目标，我的建议是将项目拆分为3个不同的层次，每层都有自己的目标，独立地工作。需要提及的是每层都使用自己的数据模型，这样才可以取得依赖（你可以看到，在代码中需要数据映射器（data mapper）来完成数据转换，不想在整个应用之上交叉使用自己的模型总要付出点代价）。下面是一个帮助你理解的模式：

![](http://jbcdn2.b0.upaiyun.com/2015/07/c1f3d18f5be40e2e949bc7b1416ac5b1.png)

注意：我没有使用任何外部库（除了解析json数据的gson，还有测试用的junit、 mockito、 robolectri和espresso）。原因是这么做会让例子会更清楚。无论如何，一定不要犹豫使用那些让生活更美好的东西，比如添加ORM来存储磁盘数据，或者任何依赖注入框架，或者任何你熟悉的工具或库。（记住，重新发明轮子不是一个好的实践）。

#### 表示层

这里实现了与视图和动画有关的逻辑。它只使用一个模型-视图-表示器（Model View Presenter， 下文称[MVP](http://en.wikipedia.org/wiki/Model%E2%80%93view%E2%80%93presenter)），但是你可以使用其他任何模式，如MVC或MVVM。在这里我不会深入介绍，这里的片段和活动只有视图，除了UI逻辑之外没有任何其他逻辑，这也是所有渲染发生的地方。这层中的表示器与交互器（用例）共同在Android UI线程之外开启新线程执行这些工作，并通过回调来处理数据，数据将在视图中渲染。

![](http://jbcdn2.b0.upaiyun.com/2015/07/f0a7ef97b9d356c4eb8d20b196239b29.png)
如果你想要一个使用MVP和MVVM更酷的[高效Android UI](https://github.com/pedrovgs/EffectiveAndroidUI/)例子，去看看我朋友Pedro Gómez所做的工作。

#### 领域层

这里的业务规则：所有逻辑都在这层。至于Android项目，你也将会看到所有的交互器（用例）实现。这层是不依赖Android的纯Java模块。所有的外部组件使用接口连接到业务对象。

![](http://jbcdn2.b0.upaiyun.com/2015/07/1255acd8de2f6705b7963620823fc1d6.png)

#### 数据层

应用需要的全部数据都来自这层，数据通过一个使用[仓储模式（Repository Pattern）](http://martinfowler.com/eaaCatalog/repository.html)实现的UserRepository存取，其策略是通过一个工厂，根据特定的条件来选择不同的数据源。比如，当通过id获取用户时，如果用户已存在于缓存中，就会选取磁盘缓存数据源，不然就会查询云端获取数据，之后保存到磁盘缓存。这里的思想是，数据的来源对客户端是透明的，它不关系数据到底来自内存、磁盘还是云端，唯一的事实是数据将会到达，然后被获取。

![](http://jbcdn2.b0.upaiyun.com/2015/07/c4dbed79c1a3c5a730c86f87eda5637e.png)

注意：我使用文件系统和Android首选项实现了一个简单的磁盘缓存，仅用于学习目的。再次提醒，如果已经存在某些库可以更好地实现这些功能，你不应该重新发明轮子。

### 错误处理

这个话题很容易引发讨论，如果你能分享自己的解决方案会更棒。我的策略是采用回调，因此，一旦数据仓库有事件发生，回调包含2个方法onResponse()和onError()。最后一个将异常封装到称作“ErrorBundle“的包装类中：这种方法会带来很多难处，因为只有一个回调链，错误会一直到达表示层被渲染。代码可读性略显不佳。另外，我也可以实现一个事件总线系统，一旦出错就抛出事件，但是这个方案就像使用GOTO一样；我认为，有时候如果你订阅了多个事件，不仔细控制的话很容易迷失。

### 测试

关于测试，我倾向于根据不同层选择多重方案：

*   表示层：使用Android instrumentation或espresso进行集成和功能测试。
*   领域层：JUnit和mockito进行单元测试。
*   数据层：Robolectric（由于这层有Android依赖）和junit、mockito进行集成和单元测试。

### 代码展示

[github链接](https://github.com/android10/Android-CleanArchitecture)

目录结构:

*   presentation：表示层的Android模块。
*   domain：无Android依赖的java模块。
*   data：所有数据存放的Android模块。
*   data-test：数据层的测试。由于使用Robolectric有些限制，我不得不在新的Java模块中使用。

### 结论

就像Bob大叔说过的，“架构在于目的而非框架”，我完全赞同。当然，有很多不同的做事方式（不同的实现），我确定你（像我一样）每天都面对很多挑战，但是使用这些技巧，你可以保证应用将会：

*   易于维护。
*   易于测试。
*   非常内聚。
*   解耦。

### 链接和资源

1.  [源代码](https://github.com/android10/Android-CleanArchitecture)
2.  [Bob大叔的整洁架构](http://blog.8thlight.com/uncle-bob/2012/08/13/the-clean-architecture.html)
3.  [架构在于目的而非框架](http://www.infoq.com/news/2013/07/architecture_intent_frameworks)
4.  [模型 视图 表示器](http://en.wikipedia.org/wiki/Model%E2%80%93view%E2%80%93presenter)
5.  [Martin Fowler仓储模式](http://martinfowler.com/eaaCatalog/repository.html)
6.  [Android设计模式演示稿](http://www.slideshare.net/PedroVicenteGmezSnch/)