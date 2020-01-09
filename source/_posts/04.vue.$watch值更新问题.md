---
title: vue.js中$watch的oldvalue与newValue
date: '2018-12-11 17:02:31'
tag: ['vue']
meta:
  -
    name: description
    content: null
  -
    name: keywords
    content: null
---
由于vue内部机制，导致对象或数组改变时旧值将与新值相同
<!-- more -->
vue.js文档中指出&nbsp;&nbsp;&nbsp;&nbsp;<font color=#42b983>[详情>>](https://cn.vuejs.org/v2/api/index.html#vm-watch)</font> 

![vueWatch](./img/vueWatch.png)

# 解决方案

## 一，this.$set 方法
## 二，Object.assign()方法
## 三，计算属性 computed
``` js{4}
export default {
  data () {
    return {
      msg: 'Highlighted!'
    }
  }
}
```