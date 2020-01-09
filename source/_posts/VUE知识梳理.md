---
title: VUE知识点梳理
date: 2020-01-09 19:41:52
tags: VUE
thumbnail: "https://user-gold-cdn.xitu.io/2020/1/7/16f7dd62944f6c43?imageView2/1/w/1304/h/734/q/85/format/webp/interlace/1"
---
## 1.对于MVVM的理解
---

**MVVM** 是 **Model-View-ViewModel** 的缩写 

`Model` : 代表数据模型，也可以在Model中定义数据修改和操作的业务逻辑。我们可以把Model称为数据层，因为它仅仅关注数据本身，不关心任何行为

`View` : 用户操作界面。当ViewModel对Model进行更新的时候，会通过数据绑定更新到View

`ViewModel` ： 业务逻辑层，View需要什么数据，ViewModel要提供这个数据；View有某些操作，ViewModel就要响应这些操作，所以可以说它是Model for View. 
总结： MVVM模式简化了界面与业务的依赖，解决了数据频繁更新
<!-- more -->
MVVM 在使用当中，利用双向绑定技术，使得 Model 变化时，ViewModel 会自动更新，而 ViewModel 变化时，View 也会自动变化。
> MVVM的缺点
> 1.Bug很难被调试: 因为使用双向绑定的模式，当你看到界面异常了，有可能是你View的代码有Bug，也可能是Model的代码有问题。
> 2.一个大的模块中model也会很大，虽然使用方便了也很容易保证了数据的一致性，当时长期持有，不释放内存就造成了花费更多的内存
>3.对于大型的图形应用程序，视图状态较多，ViewModel的构建和维护的成本都会比较高

## 2.VUE生命周期
---
<div align=center>![VUE生命周期](https://cn.vuejs.org/images/lifecycle.png)</div>

> 1. `beforeCreate` （创建前）vue实例的挂载元素$el和数据对象 data都是undefined, 还未初始化
> 2. `created (创建后)` 完成了 data数据初始化, el还未初始化
> 3. `beforeMount (载入前)` vue实例的$el和data都初始化了, 相关的render函数首次被调用。实例已完成以下的配置：编译模板，把data里面的数据和模板生成html。注意此时还没有挂载html到页面上。
> 4. `mounted (载入后)` 在el 被新创建的 vm.$el替换，并挂载到实例上去之后调用。实例已完成以下的配置：用上面编译好的html内容替换el属性指向的DOM对象。完成模板中的html渲染到html页面中。此过程中进行ajax交互
> 5. `beforeUpdate (更新前)` 在数据更新之前调用，发生在虚拟DOM重新渲染和打补丁之前。可以在该钩子中进一步地更改状态，不会触发附加的重渲染过程。
> 6. `updated （更新后）` 在由于数据更改导致的虚拟DOM重新渲染和打补丁之后调用。调用时，组件DOM已经更新，所以可以执行依赖于DOM的操作。然而在大多数情况下，应该避免在此期间更改状态，因为这可能会导致更新无限循环。该钩子在服务器端渲染期间不被调用。
> 7. `beforeDestroy`  (销毁前） 在实例销毁之前调用。实例仍然完全可用。
> 8. `destroyed` (销毁后） 在实例销毁之后调用。调用后，所有的事件监听器会被移除，所有的子实例也会被销毁。该钩子在服务器端渲染期间不被调用。


## 3.Vue实现数据双向绑定的原理
---
vue.js 是采用数据劫持结合发布者-订阅者模式的方式，通过Object.defineProperty()来劫持各个属性的setter，getter，在数据变动时发布消息给订阅者，触发相应的监听回调

>缺陷 
> 1. Object.defineProperty 无法监听数组变化，通过劫持push()pop() shift() unshift() splice() sort() reverse()
> 2. Object.defineProperty 只能劫持对象的属性,因此需要对每个对象的每个属性进行遍历，如果属性值也是对象那么需要深度遍历

Proxy可以直接监听数组的变化

Proxy可以直接监听对象而非属性


## 4.vue-router 路由守卫
---
`全局守卫`

>1. router.beforeEach 全局前置守卫 进入路由之前
>2. router.beforeResolve 全局解析守卫(2.5.0+) 在beforeRouteEnter调用之后调用
>3. router.afterEach 全局后置钩子 进入路由之后

`路由独享守卫`
- 路由定义是设置

```js
const router = new VueRouter({
      routes: [
        {
          path: '/foo',
          component: Foo,
          beforeEnter: (to, from, next) => { 
            // 参数用法什么的都一样,调用顺序在全局前置守卫后面，所以不会被全局守卫覆盖
            // ...
          }
        }
      ]
    })
```

`路由组件内的守卫`

>  1. beforeRouteEnter 进入路由前, 在路由独享守卫后调用 不能 获取组件实例 this，组件实例还没被创建
>  2. beforeRouteUpdate (2.2) 路由复用同一个组件时, 在当前路由改变，但是该组件被复用时调用 可以访问组件实例 this
>  3. beforeRouteLeave 离开当前路由时, 导航离开该组件的对应路由时调用，可以访问组件实例 this

## Vue的路由实现：hash模式 和 history模式
---

### hash模式：

在浏览器中符号“#”，#以及#后面的字符称之为hash，用window.location.hash读取；

特点：hash虽然在URL中，但不被包括在HTTP请求中；用来指导浏览器动作，对服务端安全无用，hash不会重加载页面。

hash 模式下，仅 hash 符号之前的内容会被包含在请求中，因此对于后端来说，即使没有做到对路由的全覆盖，也不会返回 404 错误。

### history模式：

history采用HTML5的新特性；且提供了两个新方法：pushState（），replaceState（）可以对浏览器历史记录栈进行修改，以及popState事件的监听到状态变更。

history 模式下，前端的 URL 必须和实际向后端发起请求的 URL 一致，如 http://www.xxx.com/items/id。

后端如果缺少对 /items/id 的路由处理，将返回 404 错误。

Vue-Router 官网里如此描述：“不过这种模式要玩好，还需要后台配置支持……所以呢，你要在服务端增加一个覆盖所有情况的候选资源：

如果 URL 匹配不到任何静态资源，则应该返回同一个 index.html 页面，这个页面就是你 app 依赖的页面。

## 组件之间的传值通信
---

### 1. 父组件给子组件传值
> 使用props，父组件可以使用props向子组件传递数据

```js
// parent
<child :msg="message"></child>
// child
export default {
    props: {
        msg: {
            type: String,
            required: true
        }
    }
}
```

### 2. 子组件向父组件通信

```js
// 父组件vue模板
<template>
    <child @msgFunc="func"></child>
</template>

<script>
import child from './child.vue';
export default {
    components: {
        child
    },
    methods: {
        func (msg) {
            console.log(msg);
        }
    }
}
</script>

```
```js
// 子组件vue模板
<template>
    <button @click="handleClick">点我</button>
</template>

<script>
export default {
    props: {
        msg: {
            type: String,
            required: true
        }
    },
    methods () {
        handleClick () {
            //........
            this.$emit('msgFunc');
        }
    }
}
</script>
```
### 3. 非父子, 兄弟组件之间通信
> 使用Bus 或 vuex
## VUEX

> Vuex 是一个专为 Vue.js 应用程序开发的状态管理模式。
> 有 5 种，分别是 state、getter、mutation、action、module
> vuex 的 store 是什么？
> vuex 就是一个仓库，仓库里放了很多对象。其中 state 就是数据源存放地，对应于一般 vue 对象里面的 datastate 里面存放的数据是响应式的，vue 组件从 store 读取数据，若是 store 中的数据发生改变，依赖这相数据的组件也会发生更新它通过 mapState 把全局的 state 和 getters 映射到当前组件的 computed 计算属性
> vuex 的 getter 是什么？
> getter 可以对 state 进行计算操作，它就是 store 的计算属性虽然在组件内也可以做计算属性，但是 getters 可以在多给件之间复用如果一个状态只在一个组件内使用，是可以不用 getters
> vuex 的 mutation 是什么？
> 更改Vuex的store中的状态的唯一方法是提交mutation
> vuex 的 action 是什么？
> action 类似于 muation, 不同在于：action 提交的是 mutation,而不是直接变更状态action 可以包含任意异步操作
> vue 中 ajax 请求代码应该写在组件的 methods 中还是 vuex 的 action 中
> vuex 的 module 是什么？
> 面对复杂的应用程序，当管理的状态比较多时；我们需要将vuex的store对象分割成模块(modules)。

## 父子组件加载顺序
```js
 // 加载渲染过程
 >父beforeCreate->父created->父beforeMount->子beforeCreate->子created->子beforeMount->子mounted->父mounted
```
```js
 // 父组件更新过程
    父beforeUpdate->父updated
```
```js
 // 子组件更新过程
 父beforeUpdate->子beforeUpdate->子updated->父updated
```
```js
 // 销毁过程
 父beforeDestroy->子beforeDestroy->子destroyed->父destroyed
```