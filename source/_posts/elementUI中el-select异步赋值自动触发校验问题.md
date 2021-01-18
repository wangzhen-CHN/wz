---
title: elementUI中el-select异步赋值自动触发校验问题
date: 2020-12-30 13:09:49
tags:
---

> 问题：编辑时进行字段必填校验，但是该字段被异步数据改变并置空，而触发校验，页面中表现为初始状态下红框提示必填

<!-- more -->

```html
<template lang="pug">
  el-form(ref="form" :model="formData" :rules="rules")
    el-form-item(label='交货方式' prop="deliveryType")
      el-select(v-model="formData.deliveryType")
        el-option(label="自提" :value="1")
        el-option(label="过户" :value="2")
        el-option(label="配送" :value="3")
< /template>

<script>
export default {
  data() {
    return {
      rules: {
        deliveryType: { required: true, message: '请选择交货方式' }
      },
      formData: {
        deliveryType: '1'
      }
    }
  },
  created() {
    // 异步赋值
    setTimeout(() => {
      this.formData.deliveryType = ''
    }, 0)
  },
  methods: {}
}
</script>
```
> 解决：清除校验结果

```js
  created() {
    setTimeout(() => {
      this.formData.deliveryType = ''
      this.$nextTick(() => {
        this.$refs.form && this.$refs.form.clearValidate('deliveryType')
      })
    }, 0)
  },
```