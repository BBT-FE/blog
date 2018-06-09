---
title: 微信授权登陆
date: 2018-06-09 18：00
tag:
 - 出云
 - vue
 - 登陆
photos:
 - http://img1.cache.netease.com/catchpic/2/24/248D0C5C3FB011CA792D76EEF0A3E816.jpg
---

<!-- 引言（简介） -->
  vue项目 - 微信授权登陆
<!--more-->

<!-- 详细内容 -->
微信授权登录，实质上通过code换取access_token

因为我是做前端的，所以我这只写前端逻辑，webpack axios

### 通过cookie
本次cookie方式，授权实在后台处理，前端只涉及到去cookie值判断是否登陆
1. cookie 方法
```js
// helper/util.js
/* eslint-disable*/
export function getCookie(name) {
  let arr
  let reg = new RegExp('(^| )' + name + '=([^;]*)(;|$)')
  if (arr = document.cookie.match(reg)) {
    return (arr[2])
  }
  else {
    return null
  }
}
// 设置cookie,增加到vue实例方便全局调用
export function setCookie (cname, value, expiredays) {
  var exdate = new Date()
  exdate.setDate(exdate.getDate() + expiredays)
  document.cookie = cname + '=' + escape(value) + ((expiredays == null) ? '' : ';expires=' + exdate.toGMTString())
}
// 删除cookie
export function delCookie (name) {
  var exp = new Date()
  exp.setTime(exp.getTime() - 1)
  var cval = getCookie(name)
  if (cval != null) {
    document.cookie = name + '=' + cval + ';expires=' + exp.toGMTString()
  }
};

```
2.  首先在页面跳转前做token判断，可以取到token则登陆，否则去授权
解释： '/api/user/login?url=' + callbackUrl 这个地址是后台给的授权地址，应为这种方式是后台去做授权，所以说前端处理是比较方便的，就是简单的去判断cookie中是否有token，
```js
// main.js

import { getCookie } from '../helper/util'

router.beforeEach((to, from, next) => {
  const token = getCookie('FLOW-AUTH-TOKEN')
  if (process.env.NODE_ENV === 'production') {
    if (!token) {
      // 如果为获取到token去授权
      let callbackUrl = window.location.hash
      window.location.href = '/api/user/login?url=' + callbackUrl
    } else {
      next()
    }
  } else {
    next()
  }
})

```

### 通过localStorage
这种方式是通过前端请求接口去实现
1. 添加请求拦截，（这里也可以使用上面的 router.beforeEach((to, from, next){} 去实现），在这主要用请求拦截去实现

```js
// 添加请求拦截
axios.interceptors.request.use(config => {
  let AUTH_TOKEN = getItem('AUTH_TOKEN')
  if (AUTH_TOKEN) {
    config.headers['Authorization'] = AUTH_TOKEN
  }
  return config
})
// Add filter retcode
function isOk(retcode) {
  return [
    '2000000',
    '1000006'
  ].indexOf(retcode) > -1
}
// Add a response interceptor
axios.interceptors.response.use(response => {
  let resp = response.data
  let retcode = resp.retcode
  let message = resp.message
  if (isOk(retcode)) {
    let data = null
    if (retcode === '2000000') {
      data = resp.data || resp
    } else {
      data = resp
    }
    data.message = message
    data.retcode = retcode
    return data
  }
  else {
    throw Error(response.data.msg || '服务异常')
  }
}, (err) => {
  if (err && err.response) {
    let retcode = err.response.data.retcode
    if (err.response.status === '401' || err.response.status === '403' || retcode === '1000006') {
      removeItem('AUTH_TOKEN')
      let callbackUrl = encodeURIComponent(window.location.href)
      window.location.href = '/work-order-wechat/login.html#/login?from=' + callbackUrl
    }
    switch (err.response.status) {
    case 500:
      err.message = err.response.data.message || '系统异常'
      err.retcode = retcode
      break
    default:
    }
  }
  return Promise.reject(err)
})
```
在接口返回 401 403 100006的情况下跳转到登陆页，重新登陆
2. 在登陆页授权登陆 ，接口返回token存到localStorage，
```js

// code返回正则匹配
function parseUrl(url) {
  let code = /code=([^&]+)/.exec(url)
  if (code) {
    code = code[1]
  }

  let state = /state=([^&]+)/.exec(url)
  if (state) {
    state = decodeURIComponent(state[1])
  }

  let referrer = /from=([^&]+)/.exec(url)
  if (referrer) {
    referrer = referrer[1]
  }

  return {
    code,
    state,
    referrer
  }
}
// 定义跳转微信授权 获取code
async function gotoWeixin(state) {
  let url = await askWeixinAuth(state).catch(err => console.log(err.msg))
  window.location.href = url
}
// 用户登录前  要验证用户是否进行过微信授权
  async created() {
    // 解析url, 获取所需参数
    let params = parseUrl(window.location.href)
    // URL 中没有code代表不是从微信跳转过来的, 需要去微信拿到code，拿到code后直接跳转换取access_token
    if (!params.code) {
      // 传递授权后的回调地址, 跳转回来后code被携带到url参数中
      gotoWeixin(params.referrer)
    } else { // 如果有code, 则直接进行微信授权,换取access_token
      weixinAuth({
        code: params.code,
        state: params.state
      })
      .then(res => {
        if (res.retcode === '2000000') { // 回调接口，如果数据库中存在用户信息，返回AUTH_TOKEN，否则是新用户，去注册
          let AUTH_TOKEN = res.token
          setItem('AUTH_TOKEN', AUTH_TOKEN)
          let url = params.state
          if (!url) {
            url = ''
          }
          window.location.replace(url)
        } else {
          console.log(res.msg)
        }
      })
      .catch((err) => {
      })
    }
  },
  // 登陆接口
  register() {
      Bind().then(res => {
        let params = parseUrl(window.location.href)
        if (res.retcode === '2000000') {
          let AUTH_TOKEN = res.token
          setItem('AUTH_TOKEN', AUTH_TOKEN) // 返回token
          this.$toast.show('登陆成功')
          let url = params.state
          if (!url) {
            url = 'https://kuaifang.test.zhongfl.com/work-order-wechat/#/entry'
          }
          window.location.href = url
        } else {
          console.log(res.msg)
        }
      })
      .catch(err => {
        console.log(err.msg)
      })
```