---
sidebar_position: 1
title: 简介
---

> 这是一个实现了类似[vue](https://vuejs.org/)中提供的([keep-alive](https://vuejs.org/v2/guide/components-dynamic-async.html#keep-alive))功能的组件.期望让组件达到**相对静止**的效果(并不是冻结，仅仅只是保持即时状态)

![React-stillness-component intro img](/img/intro.gif)

:::info

在实现的过程中从社区吸取了很多优秀的经验，比如 [issue #12039](https://github.com/facebook/react/issues/12039)， [react-18 Offscreen](https://github.com/reactwg/react-18/discussions/19)， [在 React 中实现 keep alive](https://zhuanlan.zhihu.com/p/214166951) 等，再结合自己的实际业务场景，最终使用了 [React.createPortal API](https://reactjs.org/docs/portals.html) 与 [redux](https://redux.js.org/) 实现了这一组件.

:::

## ✨ 特点

- **极简语法**，最少仅需一个 prop
- **更新及时**，提供 `Higher-Order Component` 以及 `Hooks` 方式管理组件
- **低学习成本**，没有额外的生命周期概念，不会影响组件的组合
- **最小影响**，不会影响 react 相关 api，包括 Context、Error Boundaries 等，甚至是 SyntheticEvent
- **性能优先**，自动懒惰加载，提高整体应用性能

## 💡 做了什么?

首先，这个组件不仅仅只是为了达到缓存组件的效果，同时也要解决缓存控制的问题

- **Portal** 可以做到**将组件渲染到屏幕之外**.
- **Redux** 可以帮助我们保存实时的缓存状态

:::info

该组件并不提供额外的生命周期的钩子来解决子组件监听缓存的状态.而是通过类似[react DnD Monitors](https://react-dnd.github.io/react-dnd/docs/overview#monitors)的方式将缓存状态与业务状态相结合，自定义向下传递的 **children props** ，也是一种增强组件可组合性的方式.

:::

### 数据状态驱动

针对缓存控制，该组件提供了 [Types](basic-concepts/items-types.md#Types) 的形式，来标识某一个甚至一批组件，来指定某一类型的缓存组件并进行分类.当然，这不是必须项.

实际上，组件内部也提供了唯一性 id，用于标识组件，两者作用实际上是一致的.想在组件内部获取 id，请参考 [getStillnessId()](api/contract-state.md) .

### DOM 操作

![DOM操作](/img/real-dom.png)

该组件为`children`提供了一个定位元素，即上图中的`[data-key="__stillness-uniqueId**__"]`节点.

以及`Portal`所需的**container**元素，即上图中的`[data-key="offscreen-wrapper"]`节点.

每次**静止**与否实际上会做一次 dom 操作，移除**container**元素或插入该元素，达到显示与隐藏的 UI 效果.同时每次开始**静止**时，如果`children`中有可滚动的元素，也会自动保存其滚动位置(默认为 true)

### react API

因为使用了`Portal`，即使真实 dom 元素被移除或移入，但组件仍然可以进行事件冒泡以及相应其他`react`相关 api，并与普通组件行为一致.

### typescript

组件目前使用**typescript**进行开发，相关的类型定义请参考[Typescript 支持](get-started.md#typescript-support).

### 兼容性

- React v16.8+
- Preact v10+

<!-- ### 集成

- **react-router** 请参考[type](#getType)
- **umijs** 请参考[type](#getType)
- **nextjs** 请参考[type](#getType) -->
