---
layout: post
slug: organizing-reducers
title: 组织 reducer
date: 2017-05-26 00:00:00
comments: true
categories:
- Programing
tags:
- react
- reactjs
- react native
- redux
- reducer
- '携程'
---

3 月份的时候在携程的技术沙龙代班做了一个关于在 react native 中使用 redux 的一些经验的分享，这里整理成文字。


## 背景


我是个从 iOS 4 时代就开始 iOS 开发的原汁原味的 native iOS 程序员，现在就职于 [Strikingly](https://www.strikingly.com/)。因为公司在 web 端拥抱 react.js 的时间很早，于是在 react native 出现的时候团队内也是倍加关注，也因为我个人非常喜欢函数式、声明式的写 UI（比如 iOS 的 [Snapkit](https://github.com/SnapKit/SnapKit)/[Masonry](https://github.com/SnapKit/Masonry)），很快就投奔了 react native 的怀抱。


[Strikingly](https://www.strikingly.com/)/[上线了](https://www.sxl.cn/)，是分别服务海外市场和国内市场的一个建站平台，利用我们的建站工具，没有设计、技术背景的人，在很快的时间内就可以快速搭建一个网站。在我入职不久后，就完成开发了 Strikingly 的 native app。而上线了网站也是 16 年 4 月才上线，此时已经深受 react native 毒害的我们，在到要开发上线了 app 的时候就毫无犹豫的选择了 RN（其实还是犹豫过的，具体过程还可以写另一篇文章）。当然也采用了 redux，那时候 redux 已经几乎是一统天下了。于是一个前端 + 一个 iOS + 一个 android + 一个 CTO，花了三个月完成了上线了的 app。


app 开发过程中，我们不断在反省架构、不断迭代，三个月时间里，很多代码都因为在实践上有了更好的做法而重写了。各种迭代发生在野蛮生长的几个星期里，单针对 reducer 的组织管理，就走过不少弯路，我从这些弯路里总结出了以下几个经验。


## 1、Component 和 Reducer 不想要一一对应


上线了 app 的功能大致是，有很多网站需要你管理，可以看到每个网站的流量统计、和访客之间往来的消息，甚至如果有电商模块那么还有订单、物流信息等。拿到手我们按功能纵向（这也决定了，当我们要横向的把 reducer 层做改动的时候，每个人都要投入到对自己纵向功能的重构，有利有弊）分了个工，然后就按照之前制定的计划开始做了。


直觉上大家都是一个界面（Component）创建一个 reducer。于是网站管理有了 `SitesReducer`，订单有了 `OrdersReducer` 等等诸如此类的 reducer，可以想象，最后的结果就是这样的 。


![](/images/uploads/jekyll/reducers-1-app-reducers.jpg)

到这时候我们的反省是，网页上的 `P`, `IMG`, `DIV`，可以算是最小单元的 Component 的话，不至于他们也需要自己的 reducer 吧。一旦打通这个逻辑，重新审视我们一层一层细分下去的 component 的时候，很快就意识到，很多 reducer 是没必要存在的。


结合一个具体的例子来讲，这是我们 app 其中一组界面，消息管理，包括列表和详情两个界面。


![](/images/uploads/jekyll/reducers-2-responses.jpg)




正如我之前所说，从直觉出发，我们给这两个界面设计状态管理的时候，自然而然写成了两个 reducer —— `ResponsesReducer` 和 `ResponseDetailReducer`。两个 reducer 各自拥有自己的 props 是状态，和一些 actions 来针对这些状态做修改。`ResponsesReducer` 是这样：


```typescript
// Props
type: inbox|spam
responsesList: FormReponse[]
// Actions
fetchReponsesThunk: () => Thunk
updateList: (newList: FormResponse[]) => FSA
changeType: (newType: number) => FSA
// ...
```


`ResponseDetailReducer` 是这样。

```typescript
// Props
response: FormResponse
// Actions
replyResponseThunk: () => Thunk
deleteReponseThunk: () => Thunk
markResponseAsSpamThunk: () => Thunk
updateResponse: (newResponse) => FSA
// ...
```

看到 `ResponseDetailReducer` 的时候，有两行就让我觉得很不顺眼。`response: FormResponse` 这个属性和 `updateResponse: (newResponse) => FSA` 这个 action。这个 response 是点击列表某一个之后来的，是从 `responseList` 里拿到的，其实是同一个数据的 copy。这也要求实现 `updateResponse` 的时候，要想办法同时更新这两个 copy，但这两个 copy 实质上记录的是同一个状态，这很不 redux。


完全可以把两个 reducers 合并，并且把复制的对象改成索引，像这样：


```typescript
// Props
type: inbox|spam
responsesList: FormReponse[]
activeId: number
// Actions
fetchReponsesThunk: () => Thunk
updateList: (newList: FormResponse[]) => FSA
changeType: (newType: number) => FSA
replyResponseThunk: () => Thunk
deleteReponseThunk: () => Thunk
markResponseAsSpamThunk: () => Thunk
updateResponse: (newResponse) => FSA
```


顺眼多了，这就是我们第一个收获 UI 和 Reducer 是不需要一一对应的。


## 2、统一管理 UI 状态


但这么做还是不够好，如果再看一眼这个 reducer 的话，会注意到 `type` 和 `changeType: (newType: number) => FSA` 这一组东西，其实维护的是 UI 的状态，和数据无关。如果只是这个例子的话还算可以接受，如果在更复杂的界面，UI 的状态非常多，与之对应的 action 也非常多，那么维护这些状态和 action 的工作量就非常之大。并且和数据不同，这些 UI 状态的生命周期往往更短，也因此有很多新的需求，比如希望把关闭的 UI 的状态保存下来，再比如需要快速把 UI 变回原始的状态等等。这都要求我们对 UI 相关的状态做更加高级的管理。于是：把 UI 的状态抽出来。


我的第一反映就是，做一个这样的 `UIReducer` 放整个 app 所有的状态：


```typescript
// Props
ui: { foo: bar }
// Actions
updateUI: (key: string, value: string) => FSA
```


拿去给同事们安利…… 大家更多的还是担心 UI 状态变得很混乱，难以维护，背离了 Component 自己管理自己需要的状态的哲学。我们 CTO 郭达峰也看了，什么都没说，就丢给了我一个链接，「呐，别人早就有更优秀的实践啦」。就是它：[redux-ui](https://github.com/tonyhb/redux-ui) 。


不具体展开说怎么使用了，利用 ES6 decorator 的语法重构起来也很简单，redux-ui 可以很方便的管理你的 UI 状态，并且保持了保持了 Component 自己状态的内举，只要 key 不同。另外它也实现了诸如保存状态、还原状态等功能。具体欢迎大家移步他的文档啦。


总之接下来我们的重构，就是把 UI 相关的各种状态抽出来，交给 redux-ui 来处理。


## 3、统一管理实体相关的状态


去除了 UI 的状态，像消息管理的 reducer 里需要照顾的东西，就只剩下以下的这些和 `Response` 这个实体相关的了。


```typescript
// Props
responsesList: FormReponse[]
activeId: number
// Actions
fetchReponsesThunk: () => Thunk
updateList: (newList: FormResponse[]) => FSA
replyResponseThunk: () => Thunk
deleteReponseThunk: () => Thunk
markResponseAsSpamThunk: () => Thunk
updateResponse: (newResponse) => FSA
```


退一步想想，作为客户端，我们的代码到底在做一些什么。我粗糙的总结下来，不外乎：


* 管理实体，比如消息管理的每条信息。

* 从服务端抓取数据，并很大程度上保持了一样的数据结构。

* 发各种 GET/POST/PATCH/DELETE 的请求来保持客户端数据和服务端之间数据同步

* 在不同的界面上以特定的方式展示这些实体


尤其是最后一点，为了避免同一个实体在不同的界面中被不断的重复拷贝，其实…… 是不是可以把实体都抽出来。


于是我们开始了这方面的实践，大致是这么设计的。首先最大一层，他是一个 `entityReducer` 当然掌管了所有的实体 reducer 了。它下面，以实体为单位，又各自创建了自己的 reducer，然后用 `combineReducer` 这些 reducer 归到一起。于是我们的代码大致看起来是这个样子


![](/images/uploads/jekyll/reducers-3-entity-reducer.jpg)



每个所谓的实体 reducer，比如例子里的 `responseReducer` 里我们又分了很多个 reducer 来组织数据。首先，所有的实体都被 normalize 后压扁了以 `{key: entity}` 的方式组织在一个叫做 `data` 的 reducer 里。不论你来自什么界面，这个 response 和其他 entity 的关系如何，只要有这样一个实体，它就会被放在这一个结构下。


那么带来的问题就是，在我们的业务里，这些 `Response` 实际上隶属于不同的网站，另一个 entity `Site`，我不会希望在 site A 的列表里显示 site B 的消息。所以为了管理这样的集合，我们第二个措施，是引入了一个叫 `list` 的 reducer，这里面管理了各种以 `{listname: {ids: []}}` 状态的 reducer，另外一些副作用的中间状态、分页等，也以其他更复杂的机制放在他们一起，不展开讨论了。这么一来，虽然 `Response` 都在一起，但也能按照一定的业务逻辑分开组织了。


那么简单来说，reducer 的层级从状态上来看，数据结构是这样：


```typescript
entities: {
  response: {
    data: {
      11934: {
        id: 11934,
        comment: 'test test',
        isSpam: false,
      },
    },
    list: {
      10534123FORM_RESPONSE_TYPE_INBOX: {
        ids: [11934],
      },
    },
  },
}
```


依然有问题，这个复杂的结构根本无从管理，在你写 component 的时候，如果想在 `mapStateToProps` 里要找到正确的状态，几乎是无法完成的任务。为了解决这个问题，我们必须引入 `Selector` 。


每个实体有它自己对应的 Selector，这些 selector 最基本的功能就是记录自己的 path，通过这个 path，一定程度上解决了最基本的找到自己的功能。


因为每个界面组织实体的结构并不相同，于是 Selector 还有很多 `get` 方法，这些 get 方法往往和业务更紧密，利用上面提到的 path，业务根据自己的需求去获得、组合自己的数据，并返回。返回的数据，往往就能直接 map 到 props 来 render Component 了。比如某个 `Site` 要拿到自己 inbox 里所有 `Response`，那么 `entities - response - list - {site_id}FORM_RESPONSE_TYPE_INBOX` 就是他正确的 path 了，这种复杂的逻辑，Component 只要知道对的 Selector 就行。

所以我们的做法：

* 抽出所有实体相关的数据，和 UI 解耦

* 把操作相同实体的东西放在一起

* 在各个 UI 中共用这些状态


带来的好处主要可以归结于两点：


1. **数据的一致性**。传统 MVC 开发下，controller 间同步数据，或 notification 也好，或回调也好的方式，就完全不需要存在了。因为所有的 entity 状态只存在最多一个拷贝，所以所有关于相同实体的更新，都可以同时完成了。

2. **代码的一致性**。这是之前都没有提到过的好处，因为 entity 和服务端结构几近相同，我们完全有可能让这些代码最大程度上得到服用。桌面 web 前端，如果 API endpoints 相同的话，甚至连大部分 action（thunk 或 saga）都可以被重用。事实上我们 iOS/Android/FE 就有很大程度上的代码共享。


以上是我们对 entity 做的重构，也是第三个最重要的重构。


## 总结


结合我们的代码，其实这三点是三个阶段慢慢认识到的。而在这种重构下，再回头去看之前提到的消息界面，就不再需要再有自己的 reducer 了，好处是我们的 Component 变得完全 dumb 且 pure，逻辑都在 Component 之外。


## 讨论


我们的实践其实有很多值得讨论的地方，包括在携程分享时 Q&A 以及之后的时间里，都涉及到了一些问题，我这里列举下。


1. UI 状态为什么不直接用 this.state？

2. Component 自己的状态被分得四分五裂，那么 Component 这个内聚的概念就被破坏了？

3. action 是否需要 action creator ？（我们的实践中，因为引入了 FSA，逐渐放弃了 action creator，关于 action 的更多重构，可能下次我会再总结好做一个分享、或写成文章吧）

4. 这样的实践是不是和 Isomorphic 的实践相矛盾？（这是现场观众对我和工业聚 的分享一起思考产生的问题）
5. 有一个维护成本很高的问题在于，每次修改一个实体的时候，`data` 和各种 `list` reducer 都需要处理同一个 action。比如用户创建了一个新的 `Response` ，`dataReducer` `listReducer` 等等都要去处理 `updateResponse` 这个 action，data 要把它 normalize 放进去，list 要知道它应该在哪里，如果有不同排序的 list 那么更可怕了每个 `listReducer` 都会需要新的 `Response` 需要放在哪里。权衡下来，这种做法是否值得？


欢迎大家踊跃留言或 email 联系我讨论！


