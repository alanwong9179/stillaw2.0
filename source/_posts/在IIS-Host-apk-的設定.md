---
title: 在IIS Host .apk 的設定
catalog: false
author: swillaw
header-img: /img/home-bg.jpg
date: 2022-11-30 20:41:46
tags:
---

# 在IIS Host .apk 的設定

---

### 最近工作上有一需求，大量用戶要求下載並安裝一個 Android Apk。

### 當初是打算放在 google drive 或 shared drive 之類的地方，但因為 APP 公司內部用途，在 google drive 似乎不是太安全。 加上大多數用戶都是在手提電話下載 APP 的，shared drive 太不方便了。

### 於是便想到把 .apk 檔掛在 iis 上，用戶只需要在手機瀏覽器上輸入  URL，即可下載 .apk 並安裝。

---
 ## 先在 root 開一個 app 的文件夾，並放入 .apk 檔

<br>[<img src="https://firebasestorage.googleapis.com/v0/b/stillaw-1b875.appspot.com/o/IIS-Host-Apk%2Fdemo.png?alt=media&token=07d62950-f924-4ad1-a749-b7a5c23e04a7" width="600"/>](https://firebasestorage.googleapis.com/v0/b/stillaw-1b875.appspot.com/o/IIS-Host-Apk%2Fdemo.png?alt=media&token=07d62950-f924-4ad1-a749-b7a5c23e04a7)

<br />
<br />
<br />

## 在 iis 上選擇目標的站台， 右方選擇 MIME 類型

<br>[<img src="https://firebasestorage.googleapis.com/v0/b/stillaw-1b875.appspot.com/o/IIS-Host-Apk%2Fdemo1.png?alt=media&token=a1eb3f0a-3971-42f3-8548-514821048394" width="800"/>](https://firebasestorage.googleapis.com/v0/b/stillaw-1b875.appspot.com/o/IIS-Host-Apk%2Fdemo1.png?alt=media&token=a1eb3f0a-3971-42f3-8548-514821048394)

<br />
<br />
<br />

## 右上方點選新增，輸入 

```jsx
副檔名:
.apk

MIME 類型:
application/vnd.android.package-archive
```
<br>[<img src="https://firebasestorage.googleapis.com/v0/b/stillaw-1b875.appspot.com/o/IIS-Host-Apk%2Fdemo2.png?alt=media&token=7d8d6691-6cdf-41fd-a9b1-0d083f1715f6" width="800"/>](https://firebasestorage.googleapis.com/v0/b/stillaw-1b875.appspot.com/o/IIS-Host-Apk%2Fdemo2.png?alt=media&token=7d8d6691-6cdf-41fd-a9b1-0d083f1715f6)


## 完成