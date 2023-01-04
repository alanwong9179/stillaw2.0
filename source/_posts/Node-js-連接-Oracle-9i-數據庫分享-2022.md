---
title: Node.js 連接 Oracle 9i 數據庫分享 2022
catalog: false
author: stillaw
header-img: /img/home-bg.jpg
date: 2022-11-21 20:45:28
tags:
---
# Node.js 連接 Oracle 9i 數據庫分享 2022
---
### 引言
公司最近打算將兩套系統相互合併，其中有一部分是要利用Node.js架起Express js伺服器，然後對Oracle 數據庫進行CURD。
眼看不難，但在接駁兩套年代相差如此遠的系統上，也走了不少歪路。

### 準備

* Oracle Client Package 11.2
  11.2 是目前2022年 Oracle官方所提供的最接近9i的 Client package
  [下載連結](https://www.oracle.com/cn/database/technologies/microsoft-windows.html)
  ![AltText {priority}{768x432}](https://drive.google.com/u/0/uc?id=1_CajPGlNZ0o7G_fNuAqU0XVXdnZNVSYQ&export=download)
  
* Node.js
* ODBC
  MS的一個服務，作為Node.js及Oracle Client的中介。可以在 `控制台 > 系統及安全性 > 系統管理工具` 中找到。
  <img src="https://drive.google.com/u/0/uc?id=1UR0YpmzEYslwkIrQKXRDvtD3SAg4a8MR&export=download" width="80%">

### 步驟
1. 安裝及設定 Oracle Client 11.2
2. 接通ODBC與Oracle 9i數據庫
3. 按通Node.js與ODBC的連線

### 安裝及設定


> 安裝及設定 Oracle Client 11.2

#### 下載並解壓縮Oracle Client 11.2
 
- Installation Type 選擇 `Administrator(1.04GB)`
- 選擇`Software Location`時，要確保路徑要在Oracle Base 內

安裝完成後，我們要在安裝好的根目錄內建立一個文檔 `tnsnames.ora`
路徑為 `根目錄/network/admin (e.g. instantclient_11_2\network\admin)`

在`tnsnames.ora`加入Oracle 數據庫的設定

```javascript
CONN = 
    (DESCRIPTION =
        (ADDESS_LIST =
            (ADDRESS = (PROTOCOL = TCP) (HOST = xxx.xxx.x.x) (PORT= xxxx) )
        )
        (CONNECT_DATA = 
            (SERVICE_NAME = conn)
        )
    )
```
詳細`tnsnames.ora`的寫法也可參考 [Omiting tnsnames.ora](https://www.connectionstrings.com/oracle/)
&nbsp;

> 接通ODBC與Oracle 9i數據庫
#### 設定ODBC Client

如果前面設置正確，在ODBC Client，`驅動程式`中，會見到一個驅動程式名為 `Oracle in OraClient1g_home1`。

回到系統資料來源名稱，建立新的資料來源，選擇`Oracle in OraClient1g_home1`。 之後跟著表格填上相應的資料後按`OK`

現在，你的ODBC Client 已經成功連上Oracle 9i 數據庫。
  &nbsp;



> 按通Node.js與ODBC的連線
#### 架設Node.js 與 ODBC Client 的連線

我們會利用 `odbc` 這個插件來幫忙連線。

先安裝插件 
`npm install odbc`

設定 connectionConfig
```javascript
const connectionConfig = {
    connectionString: 'DSN=xxx;UID=userid;PWD=password;',
    connectionTimeout: 10,
    loginTimeout: 10
}
```
詳細其餘的config可參考 [odbc-npm](https://www.npmjs.com/package/odbc)

之後便可使用
```javascript
const odbc = require('odbc');

odbc.connect(connectionConfig, (error, conn) => {
    if(error){
        console.log(error)
    }

    conn.query(`SELECt * FROM table`, (err, result) => {
        if(err){
            console.log (err)
            return;
        }
        console.log(result)

    })
})

```
