---
layout: post
title: ESP32 IoT webserver & client
author: [lantien]
category: [Lecture]
tags: [jekyll, ai]
---
---
### ESP32_Webserver_IoT、ESP32_Webclient_IoT


### 應用功能說明
1. WIFI遠端控制 
2. 透過DHT11 sensor感測周圍溫度、濕度
3. server 和 client 聯機控制

### 設計考量與相關技術
**系統設計考量：**<br>
1. 操作方式:WIFI遙控手機App or 筆電
2. 元件:ESP32、DHT11 sensor 
3. 聯網方式:WI-FI

**所需相關技術：**
1. Arduino程式設計


### 系統方塊圖
![](https://github.com/hjgyjg123/MCU-project/blob/main/images/Webserver%E7%B3%BB%E7%B5%B1%E6%96%B9%E5%A1%8A%E5%9C%96.jpg?raw=true)


### webserver程式碼
![](https://github.com/hjgyjg123/MCU-project/blob/main/images/webserver%2001.jpg?raw=true)
![](https://github.com/hjgyjg123/MCU-project/blob/main/images/webserver_IOT%202.jpg?raw=true)
![](https://github.com/hjgyjg123/MCU-project/blob/main/images/webserver_IOT%203.jpg?raw=true)
![](https://github.com/hjgyjg123/MCU-project/blob/main/images/webserver_IOT%204.jpg?raw=true)
![](https://github.com/hjgyjg123/MCU-project/blob/main/images/webserver_IOT%205.jpg?raw=true)
![](https://github.com/hjgyjg123/MCU-project/blob/main/images/webserver_IOT%206.jpg?raw=true)

### webclient程式碼
![](https://github.com/hjgyjg123/MCU-project/blob/main/images/webclient%2001.jpg?raw=true)
![](https://github.com/hjgyjg123/MCU-project/blob/main/images/webclient%2002.jpg?raw=true)

### 結果
![](https://github.com/hjgyjg123/MCU-project/blob/main/images/webserver%202.jpg?raw=true)
![](https://github.com/hjgyjg123/MCU-project/blob/main/images/webserver%203.jpg?raw=true)
![](https://github.com/hjgyjg123/MCU-project/blob/main/images/webserver%204.jpg?raw=true)
![](https://github.com/hjgyjg123/MCU-project/blob/main/images/webserver%201.jpg?raw=true)

---
<br>
<br>

This site was last updated {{ site.time | date: "%B %d, %Y" }}.
