---
title: '[Framer Motion] 拖拽按鈕'
catalog: false
author: stllaw
header-img: /img/home-bg.jpg
date: 2022-12-16 20:46:20
tags:
---

# [Framer Motion] 做一個拖拽按鈕
---

## 引言
自從智能手機高速普及後，frontend developer 們也為了移動端的用戶能有更好的UX而不斷開發出新的操作方法。 其中，拖拉處理按鈕動作更是潮流。

當年iPhone 1 的發佈會上，Steve Jobs 示範用拖拽來解鎖手機，更成為手機界的一時佳話，同時也引來不少同業仿傚。

[![iphone 1 unlock](https://img.youtube.com/vi/e2cKG9XcE7c/0.jpg)](https://www.youtube.com/watch?v=e2cKG9XcE7c)


時至今天，市面上有不少免費的動畫library提供，令我們可以輕鬆地做出漂亮的效果。

#
#
## 回到正題，我們今天將會使用 ```framer-motion``` 在 ```React``` 上做一出個拖拉按鈕。
![demo](https://firebasestorage.googleapis.com/v0/b/stillaw-1b875.appspot.com/o/Framer-Motion-Drag-Button%2Fdemo.gif?alt=media&token=9ee25434-ce6d-4474-a81d-e3379191004c)

### 先安裝Framer Motion 
 ```npm install framer-motion```


### 按鈕的JSX 及 CSS

``` jsx
import { motion } from "framer-motion";

 <div id="container">
      <div id="outer">
        <motion.div id="inner"></motion.div>
      </div>
    </div>
```

``` css
#container {
    display: flex;
    background-color: #333333;
    height: 100vh;
    align-items: center;
    justify-content: center;
}

#outer {
    width: 300px;
    height: 100px;
    border-radius: 10px;
    background-color: rgb(236, 236, 236);
}

#inner { 
    width: 100px;
    height: 100%;
    border-radius: 10px;
    background-color: #8EC5FC;
    background-image: linear-gradient(62deg, #8EC5FC 0%, #E0C3FC 100%);
}
```

此時，我們得到了一個按鈕，我們將會控制 ```#inner``` 來達到拖拉效果。

### 加入拖拉定義
 ```drag="x"```
```jsx
<motion.div 
drag="x" //<--here!
id="inner"
></motion.div>
```

drag令它可以用cursor來拖拉了，意味著我們也成功了50%。 
drag 可以接受 ```x | y | boolean``` ， ```x``` 及 ```y``` 分別代表水平及垂直移動，```boolean``` 則是代表是否啟用drag功能。

### 解決回彈，慣性
framer motion 是一個非常真實的庫，她注重物理的原則，所以基於慣性，我們的按鈕會馬上飛走，就像在太空一樣 xd
![demo](https://firebasestorage.googleapis.com/v0/b/stillaw-1b875.appspot.com/o/Framer-Motion-Drag-Button%2Fdemo2.gif?alt=media&token=e83296c9-877b-4b50-960f-192db0c4eecd)

我們需要這個按鈕像彈弓一樣，如果不到達目的地就放手的話，它要回彈到起始位置。

加入 ```dragSnapToOrigin={true}```
```jsx
<motion.div
dragSnapToOrigin={true}//<--here!
...
></motion.div>
```
完成回彈效果
![demo](https://firebasestorage.googleapis.com/v0/b/stillaw-1b875.appspot.com/o/Framer-Motion-Drag-Button%2Fdemo3.gif?alt=media&token=248879f6-01d9-482f-a312-ccf0e5a54bf1)

### 限制位置
我們希望用戶向右拉來解鎖，所以我們要禁止向左拉。

```jsx
<motion.div
dragConstraints={{ left: 0, right: 200 }}//<--here!
dragElastic={0}//<--here!
...
></motion.div>
```

`dragConstraints` 用於限制按鈕的拖拉範圍， `dragElastic` 代表範圍有多嚴謹，`0` 就是代表完全不可動了。

![demo](https://firebasestorage.googleapis.com/v0/b/stillaw-1b875.appspot.com/o/Framer-Motion-Drag-Button%2Fdemo4.gif?alt=media&token=008a7664-4030-4dd7-9db1-fbcc94c6399f)

### 完成拖拉指示
我們定義按鈕的完成位置，當按鈕被拉到最右方，即觸發事件。

```jsx
const [dragDone, setDragDone] = useState(false)

<motion.div
...
//drag="x"
drag={dragDone ? false :"x"}
onDrag={(event, info) =>  
{info.offset.x > 200 && setDragDone(true)}}//<--here!
></motion.div>
```

`onDrag` event 會告訴我們目前拖拉的狀態，在這個例子中，我們的按鈕長`300px`，而可拖拉的部份為`100px`，所以我們 `info.offset.x` 大於 `200px`，即代表到達。

只要用`useState`來存放完成的狀態，並將其加入`drag`即可完成。當`dragDone`為true，`drag`將為`false`。一但`drag`是`false`，所有有關`drag`的活動將會停止。

![demo](https://firebasestorage.googleapis.com/v0/b/stillaw-1b875.appspot.com/o/Framer-Motion-Drag-Button%2Fdemo.gif?alt=media&token=9ee25434-ce6d-4474-a81d-e3379191004c)


### Reference
https://www.youtube.com/watch?v=e2cKG9XcE7c
https://www.framer.com/docs/gestures/#drag