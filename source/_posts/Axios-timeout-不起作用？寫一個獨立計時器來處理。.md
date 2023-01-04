---
title: Axios timeout 不起作用？寫一個獨立計時器來處理。
catalog: false
author: stillaw
header-img: /img/home-bg.jpg
date: 2022-12-16 20:46:52
tags:
---

## Axios timeout 不起作用？寫一個獨立計時器來處理。
---
Axios 的詳細用法可以按[這里](https://github.com/axios/axios)了解更多。

最近需要在一條 API CALL 中加入時間的限制，目的是在三秒內如果 API 沒有任何回應就強行中斷。

嘗試過其他方法，例如在 axios call 中加入 ```Timeout``` 參數，但沒有用。

然而，Axios 本身官方有提供一個名叫 ```cancelToken``` 的參數供用戶強制中斷請求。



是次的例子環境為 ```React Native```, 先用NetInfo插件檢查網絡狀況。

```js
npm install @react-native-community/netinfo
```


```js
NetInfo.fetch().then(async state => {
 if(!state.isConnected){
   // not network
   return
 }else{
  await axios.post(`https://xxxx/api/xxx`,
  {user:'Alan'})
    .then(response => {
     //normal flow
   }).catch((error) => {
     //error
   })
 }
})
```

### 定義一個 `CancelToken`
```js
const CancelToken = axios.CancelToken;
```
### NetInfo檢查完後加入 setTimeout，今次例子為三秒。
```js
setTimeout(() => {
  cancelSource.current.cancel()
}, 3000)
```
### 在請求串中加入 cancelToken 指令
```js
 await axios.post(`https://xxxx/api/xxx`,
  {user:'Alan'},
  { cancelToken: cancelSource.current.token} // <-- right here
  )
    .then(response => {
     //normal flow
   }).catch((error) => {
     //error
   })
```
### 如此一來，如果限時內Axios沒有回應，就會跳Catch。

## 注意
此操作對於backend的checking功能比較嚴謹。
