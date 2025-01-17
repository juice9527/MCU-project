---
layout: post
title: 藍牙遙控機器人實作
author: [lantien liang]
category: [homework]
tags: [jekyll, ai]
---

This project is to implement a bluetooth remote controlled robotcar.


## 藍牙遙控機器人
![](https://github.com/rkuo2023/MCU-project/blob/main/images/ESP32_RoboCar.jpg?raw=true)

### 應用功能說明
1. Bluetooth remote control App 
2. two-wheel robocar


### 設計考量與相關技術
**系統設計考量：**<br>
1. 操作方式: 藍牙遙控手機App
2. 移動方式: 兩輪 
3. 供電方式: 鋰電池 3.7V x2
4. 聯網方式: 藍牙

所需相關技術：
1. MIT App Inventor 2. 手機程式設計 
2. Arduino程式設計



所需相關套件:
![](https://image.ruten.com.tw/g2/8/d4/16/21440347657238_872.jpg?raw=true)

### 系統方塊圖
![](https://github.com/juice9527/MCU-project/blob/main/images/341759047_182647211360607_2697958425567335018_n.jpg?raw=true)

### 實作影片

<iframe width="320" height="560" src="https://www.youtube.com/embed/y1cQ3dTrR-g" title="2023年4月20日" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>

### 程式碼
// PWM to DRV8833 dual H-bridge motor driver, PWM freq. = 1000 Hz
// ESP32 Webserver to receive commands to control RoboCar

#include <WiFi.h>
#include <WebServer.h>
#include <ESP32MotorControl.h> 

//定義馬達驅動腳位
// DRV8833 pin connection
#define IN1pin 16  
#define IN2pin 17 
#define IN3pin 18 
#define IN4pin 19

//定義左右馬達代號與速度
#define motorR 0
#define motorL 1
#define FULLSPEED 100
#define HALFSPEED 50

初始化馬達
ESP32MotorControl motor;


/* 設定WIFI連線 */
const char *ssid = "Galaxy A42 5G148D";
const char *password = "hjgyjgabc";

WebServer server(80); // Set web server port number to 80

//HTML網頁樣式
const String HTTP_PAGE_HEAD = "<!DOCTYPE html><html lang=\"en\"><head><meta name=\"viewport\" content=\"width=device-width, initial-scale=1, user-scalable=no\"/><title>{v}</title>";
const String HTTP_PAGE_STYLE = "<style>.c{text-align: center;} div,input{padding:5px;font-size:1em;}  input{width:90%;}  body{text-align: center;font-family:verdana;} button{border:0;border-radius:0.6rem;background-color:#1fb3ec;color:#fdd;line-height:2.4rem;font-size:1.2rem;width:100%;} .q{float: right;width: 64px;text-align: right;} .button1 {background-color: #4CAF50;} .button2 {background-color: #008CBA;} .button3 {background-color: #f44336;} .button4 {background-color: #e7e7e7; color: black;} .button5 {background-color: #555555;} </style>";
const String HTTP_PAGE_SCRIPT = "<script>function c(l){document.getElementById('s').value=l.innerText||l.textContent;document.getElementById('p').focus();}</script>";
const String HTTP_PAGE_BODY= "</head><body><div style='text-align:left;display:inline-block;min-width:260px;'>";
const String HTTP_PAGE_FORM = "<form action=\"/cmd1\" method=\"get\"><button class=\"button1\">Forward</button></form></br><form action=\"/cmd2\" method=\"get\"><button class=\"button2\">Backward</button></form></br><form action=\"/cmd3\" method=\"get\"><button class=\"button3\">Right</button></form></br><form action=\"/cmd4\" method=\"get\"><button class=\"button4\">Left</button></form></br><form action=\"/cmd5\" method=\"get\"><button class=\"button5\">Stop</button></form></br></div>";
const String HTTP_WEBPAGE = HTTP_PAGE_HEAD + HTTP_PAGE_STYLE + HTTP_PAGE_SCRIPT + HTTP_PAGE_BODY + HTTP_PAGE_FORM;
const String HTTP_PAGE_END = "</div></body></html>";

// Current time
unsigned long currentTime = millis();
// Previous time
unsigned long previousTime = 0; 
// Define timeout time in milliseconds (example: 2000ms = 2s)
const long timeoutTime = 2000;

//設定馬達速度
int speed = HALFSPEED;

void handleRoot() {
  String s  = HTTP_WEBPAGE; 
  s += HTTP_PAGE_END;
  server.send(200, "text/html", s);
}

//移動前進
void cmd1() {
  String s  = HTTP_WEBPAGE; 
  s += HTTP_PAGE_END;  
  server.send(200, "text/html", s);
  motor.motorForward(motorR, speed);  
  motor.motorForward(motorL, speed);
  Serial.println("Move Forward");   
}

//移動後退
void cmd2() {
  String s  = HTTP_WEBPAGE; 
  s += HTTP_PAGE_END;  
  server.send(200, "text/html", s);
  motor.motorReverse(motorR, speed);
  motor.motorReverse(motorL, speed);
  Serial.println("Move Backward");     
}

//向右轉
void cmd3() {
  String s  = HTTP_WEBPAGE; 
  s += HTTP_PAGE_END;  
  server.send(200, "text/html", s);
  motor.motorReverse(motorR, speed);  
  motor.motorForward(motorL, speed);
  Serial.println("Turn Right");    
}

//向左轉
void cmd4() {
  String s  = HTTP_WEBPAGE; 
  s += HTTP_PAGE_END;  
  server.send(200, "text/html", s);
  motor.motorForward(motorR, speed);
  motor.motorReverse(motorL, speed);
  Serial.println("Turn Left"); 
}

//停止
void cmd5() {
  String s  = HTTP_WEBPAGE; 
  s += HTTP_PAGE_END;  
  server.send(200, "text/html", s);
  motor.motorStop(motorR);
  motor.motorStop(motorL);
  Serial.println("Motor Stop");
}

void setup() {
//開啟序列通訊
  Serial.begin(115200);
  Serial.println("Motor Pins assigned...");
  //設定馬達輸出引腳
  motor.attachMotors(IN1pin, IN2pin, IN3pin, IN4pin);
  
  // Connect to Wi-Fi network with SSID and password
  Serial.print("Connecting to ");
  Serial.println(ssid);
  WiFi.begin(ssid, password);
  while (WiFi.status() != WL_CONNECTED) {
    delay(500);
    Serial.print(".");
  }
  // Print local IP address and start web server
  Serial.println("");
  Serial.println("WiFi connected.");
  Serial.println("IP address: ");
  Serial.println(WiFi.localIP());

  server.on("/", handleRoot);
  server.on("/cmd1", cmd1);
  server.on("/cmd2", cmd2);
  server.on("/cmd3", cmd3);
  server.on("/cmd4", cmd4);  
  server.on("/cmd5", cmd5);  

  Serial.println("HTTP server started");
  server.begin();

  motor.motorStop(motorR);
  motor.motorStop(motorL);
}

void loop() {
  server.handleClient();
}

<br>
<br>

This site was last updated {{ site.time | date: "%B %d, %Y" }}.

