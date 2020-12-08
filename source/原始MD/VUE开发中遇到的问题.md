---
title: VUE开发中遇到的问题
date: 2020-01-09 18:18:40
tags: VUE
---
# 对象数组深度监听
>问题: 后端传过来的数组是一个数组对象，页面中绑定对象中某一具体的属性，当该值变化时调用某个函数，自然想到就是watch方法。但如何watch数组对象中某一个具体的属性，显然不可能一个个属性写watch。

解决办法：
1. watch整个对象，设置deep为true，当该对象发生改变时，调用处理函数。

2. 将页面中绑定的属性写在computed函数中，watch这个computed中的函数，当对象值改变时会进入computed函数中，进而进入watch函数中，再调用处理函数。
<!-- more -->
# 子组件生命周期执行顺序问题

组件的调用顺序都是先父后子,渲染完成的顺序是先子后父。

组件的销毁操作是先父后子，销毁完成的顺序是先子后父。

加载渲染过程
父beforeCreate->父created->父beforeMount->子beforeCreate->子created->子beforeMount- >子mounted->父mounted

子组件更新过程
父beforeUpdate->子beforeUpdate->子updated->父updated

父组件更新过程
父 beforeUpdate -> 父 updated

销毁过程
父beforeDestroy->子beforeDestroy->子destroyed->父destroyed


# 本地开发跨域问题
>问题:  在本地开发请求后端服务器接口的时候，都不可避免的会遇到跨域的问题。

解决方法可以通过加一个node中间层或者nginx做反向代理。

如果是用vue-cli搭建的项目，vue-cli在config中自带了一个proxyTable属性，可以配置这个属性解决跨域的问题。

# 父组件控制子组件样式问题

使用 xx /deep/ xx 或者 xx >>> xx

# 对象赋值无法双向绑定

例如对象obj ={a:1},如果想要修改obj中的a属性，通过obj.a = 2这样赋值，页面不会更新，需使用vue.set方法更改才会起作用， Vue.set(this,obj,a,2);

同样，如果要给obj增加一个新属性，如果该属性未在data中声明，页面也不会刷新。也就是vue文档中声明的“Vue 不能检测到对象属性的添加或删除”，同样需要使用vue.set方法进行赋值才好使。

# Vue事件总线（eventBus）$on()会多次触发解决办法

>问题:注册的总线事件（Bus）,页面没有强制刷新，存在组件切换，bus.$on方法会被多次绑定，造成触发一次但多个响应的情况

解决办法就是在利用$on 接收事件的组件的beforeDestroy或destroy周期中将事件进行销毁，使用$off()
```js
beforeDestroy () {
　　bus.$off('BUS_NAME')
}
```
附上github上Vue作者尤大大关于这问题的解答：
https://github.com/vuejs/vue/issues/3399