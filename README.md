## wx-axios-promise
### 特征
* 提供和axios相似体验，支持默认参数配置，拦截功能，和create创建新的对象
* 默认将小程序的api封装成Promise，通过降级，兼容低版本手机系统
### 使用方法
npm i wx-axios-promise -S

[小程序 npm 支持](https://developers.weixin.qq.com/miniprogram/dev/devtools/npm.html?search-key=npm)

[小程序代码片段测试](https://developers.weixin.qq.com/s/GSiAP0mj787V)


```
import Abi from 'wx-axios-promise'
let api = Abi()
```
传递相关配置来创建请求(以下参数为默认)
```
//详情可参考wx.request
let api = Abi({
    url: '',//默认的接口后缀
    method: 'get',//默认的HTTP 请求方法
    dataType: 'json',//默认的返回类型
    responseType: 'text',
    header: {
      'content-type': "application/json"
    }
  })
```
除上面的创造方法外，我们还可以用实例上的create的方法创建新实例。
```
let api = Abi()
let newApi = api.create()
```
请求操作
```
/**
*默认是get
*如果你设置了默认的url。会自动配置 默认url + url
*如果你的url是http://或者https://开头，那么不会添加默认url
*/
//多种请求方式
api(url, data)
api(SERVER[api], apiData)
api.get(SERVER[api], apiData)
api(SERVER.URL + SERVER[api], apiData)
api(`${SERVER[api]}?page=${apiData.page}&count=${apiData.count}`)
api({
   url: SERVER[api],
   data: apiData,
   <!--method: 'get',-->
   <!--dataType: 'json',-->
   <!--responseType: 'text',-->
   <!--header: {-->
   <!--    content-type': "application/json"-->
   <!--}-->
})

api.post(url, data)
支持
'get',
'post',
'put',
'delete',
'options',
'head',
'trace',
'connect'
```
可以架起请求、响应、成功、失败的拦截
```
api.interceptors.response.use(function (config){
  //接口||wx.接口
  return config.data || config
}, function(error){
    return error
})
api.interceptors.request.use(function (config){
  //返回的是和wx.request相关的参数
  console.log(config)
  wx.showLoading({
    title: '加载内容'
  })
}, function(error){
    return error
})
```
wx全Promise
```
api.wx.chooseImage()
.then( res => api.wx.uploadFile())
.then()
```
当然，如果你并不需要这个功能，你也可以在创建的时候设置第二个参数为false
