---
title: 'React Native Android Build 失敗, Oct 31, 2022 jcenter 出事了。'
catalog: false
author: stillaw
header-img: "https://speedmedia.jfrog.com/08612fe1-9391-4cf3-ac1a-6dd49c36b276/https://media.jfrog.com/wp-content/uploads/2021/02/02195850/Bintraysunsetblogb-03.png/mxw_1024,f_auto"
date: 2022-11-21 20:44:06
tags:
---

# React Native Android Build 失敗, Oct 31, 2022 jcenter 出事了。

當日早上回到公司，準備把前幾天準備好的RN APP Build 起來上架 google play。

Console 不斷出 error，一番折騰後，發現到 dependency 們都在 jcenter() 這一句跳出錯誤，找不到資源。

打開browser輸入	https://jcenter.bintray.com/ ， 網頁會顯示403 Forbidden。 怪不得dependency都build不起來了。

但為何事出如此突然呢? 其實早在一年前的二月，JFrog 平台就宣佈在同年的五月一日，將會停止一切有關jcenter的服務。

> To streamline the productivity of the JFrog Platform we will be sunsetting Bintray, GoCenter, and ChartCenter services on May 1st, 2021.

其後在2021年的四月尾，再宣佈不會下線所有jCenter的repo，而是改為唯讀。

但在一年後的今天，所有repo也突然消失。

## 影響
那麼，為什麼jCenter的消失，會令我們的App build不起來呢 ?

我們在build .apk | .aab 時，至少有兩個地方需要存取jCenter上的資料

1. 打開 ``` root/android/build.gradle ```，你會發現 ```repositories``` 中有一句 ```jcenter() ```
   
2. 在你的 ```node_modules``` 中，有著大大小小不同的library，它們有一些也會用到 ``` jcenter() ```。 例如是 ``` react-native-camera ``` ，打開 ```node_modules/react-native-camera/android/build.gradle```，會看到以下語句
 ``` javascript
 repositories {
      google()
      jcenter()
    }
```

但凡有用到```jcenter()```服務的文件，在build的時候就會因為取不到所需的data而報錯。

## 解決方法
臨時的解決方法，可以在所有的```jcenter()```上方多加一句```mavenCentral()```
同上面的camera library 作例子

```javascript 
 repositories {
      google()

      mavenCentral() // <-- add this.
      jcenter()
    }
```

長遠來說，還是要等到官方有消息公佈啊


---
##### refercence
https://stackoverflow.com/questions/74258160/is-jcenter-down-permanently-31-oct/74258237#74258237

https://jfrog.com/blog/into-the-sunset-bintray-jcenter-gocenter-and-chartcenter/


