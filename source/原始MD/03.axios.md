---
title: axios
date: '2019/01/17 17:59:40'
type: post
tag: null
meta:
  - name: description
    content: null
  - name: keywords
    content: null
---
## axios
<!-- more -->
```js
axios({
  method: 'post',
  url: 'xxxxxx',
  data: {
    a: 'data1',
    b: 'data2'
  }
})
  .then(function(response) {
    console.log(response) //请求成功
  })
  .catch(function(error) {
    console.log(error) //请求失败
  })
```

### 执行 GET 请求

```js
// 为给定 ID 的 user 创建请求
axios
  .get('/user', {
    params: {
      ID: 12345
    }
  })
  .then(function(response) {
    console.log(response)
  })
  .catch(function(error) {
    console.log(error)
  })
```

```js
### 执行 POST 请求

axios.post('/user', {
  ID: '1234'
})
.then(function (response) {
  console.log(response);
})
.catch(function (error) {
  console.log(error);
});
```
