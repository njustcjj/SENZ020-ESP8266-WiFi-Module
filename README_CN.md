# SENZ020 ESP8266 ESP-01S WiFi模块

###### 翻译

> `英文` 请参考 [`这里`](https://github.com/njustcjj/SENZ020-ESP8266-WiFi-Module/blob/master/README.md)

> `中文` 请参考 [`这里`](https://github.com/njustcjj/SENZ020-ESP8266-WiFi-Module/blob/master/README_CN.md)

![](https://github.com/njustcjj/SENZ020-ESP8266-WiFi-Module/blob/master/pic/SENZ020.jpg "SENZ020")
 

### 产品介绍

> SENZ020 ESP8266 ESP-01S WiFi模块 拥有强大的片上处理和存储能力，内置32位处理器，内置Lwip协议栈。支持AP+STA模式共存，可通过AT指令配置各种参数。
> 适用于各种3.3V的单片机系统，可用于扩展Arduino, 实现无线数据传输，远程控制。
>
> 应用：家用电器、工业无线控制、智能插座/灯、Mesh网络、传感器网络、可穿戴电子产品、无线位置感知设备、安全ID标签、无线定位系统信标

### 产品参数

- 工作模式：STA/ soft-AP/ STA + soft-AP
- 内置 TCP/IP 协议栈
- 内置低功率 32 位 CPU：可以兼作应用处理器
- 支持 WEP/WPA-PSK/WPA2–PSK加密
- 支持UART、IIC、GPIO接口
- 工作电压：3.3V
- 无线标准:IEEE802.11b/g/n
- 频率: 2.4 GHz
- 尺寸：24.7 x 14.4mm

### 使用教程

#### 引脚定义

|Sensor pin|Ardunio Pin|Function Description|
|-|:-:|-|
|VCC|+3.6~6V|Power shouldn't be higher than 7V|
|GND|GND||
|RXD|TX|Receive data|
|TXD|RX|Transmit date|
|RST|-|Reset|
|EN|+3.3V|Enable|
|GPIO0|||
|GPIO2|||


![](https://github.com/njustcjj/SENZ020-ESP8266-WiFi-Module/blob/master/pic/SENZ020_pin.jpg "引脚定义") 


#### 连线图

![](https://github.com/njustcjj/SENZ020-ESP8266-WiFi-Module/blob/master/pic/SENZ020_connect.PNG "连线图") 


### 示例代码

#### Sample1:

> To develop with serial AT command.  
> Download description for [`AT Command Doc`](http://github.com/njustcjj/SENZ020-ESP8266-WiFi-Module/blob/trunk/doc/AT Command Doc.pdf)

	/*
	Built the Serial by Arduino's Pin 2 & Pin 3, debug  serial and test the TCP/IP with other software.
	*/

	#include <SoftwareSerial.h>

	SoftwareSerial mySerial(3, 2); // RX TX

	void setup() {
	    Serial.begin (115200);
	    while (!Serial) {;}
	    Serial.println("hardware serial!");
	    mySerial.begin(115200);
	    mySerial.println("software serial!");
	}

	void loop() {

	    if ( mySerial. available()) {
	       Serial.write(mySerial.read());
	  }

	    if (Serial.available()) {
	        mySerial.write(Serial.read());
	    }
	}

---

#### Sample2:

> To develop with Arduino board.  
> Download and add the library to Arduino  IDE [`uartWIFI.h`](http://github.com/njustcjj/SENZ020-ESP8266-WiFi-Module/blob/trunk/lib/uartWIFI.zip)

	#include "uartWIFI.h"
	#define SSID "xxxxxxx"        //wifi热点
	#define PASSWORD "xxxxxxx"        //wifi热点密码
	#define LED 13 //状态指示灯

	WIFI wifi;

	/*
	 * @Desc 初始化工作
	 */
	void setup() {
	  pinMode(LED,OUTPUT);
	  initESP8266();
	  DebugSerial.println("Init end");
	}

	void loop() {
	  // put your main code here, to run repeatedly:

	}

	/*
	 * @Desc 初始化8266模块 
	 * 第一步：配置串口
	 * 第二步：配置wifi8266模块
	 *    1) AT+RST 重新启动模块 返回：OK 以及一堆乱码，忽略乱码
	 *    2) AT+CWMODE=1 以及 AT+RST 设置STA模式（重启后生效(AT+RST)) 返回：OK
	 *    3) AT+CWJAP="SSID","PASSWORD"  配置wifi账号密码(填你自己的wifi热点)
	 *    4) AT+CIPMUX=0    0单路连接模式,1多路连接模式
	 *    5) AT+CIFSR 获取本模块ip以及MAC地址
	 *    
	 */
	void initESP8266(){
	  wifi.begin(9600,9600);
	  bool b = wifi.Initialize(STA, SSID, PASSWORD);
	  if(!b){
	    DebugSerial.println("Init error");
	  }

	  wifi.confMux(0);
	  delay(100);
	  String ipstring = wifi.showIP();
	  DebugSerial.println(ipstring);        //show the ip address of module
	  DebugSerial.println("Server is set up");
	}

	void(* resetFunc) (void) = 0; //制造重启命令

	void esp8266Error(String error)
	{
	    DebugSerial.print("ERROR:");
	    DebugSerial.println(error);
	    while (1)
	    {
	        digitalWrite(LED, HIGH);
	        delay(500);
	        digitalWrite(LED, LOW);
	        delay(500);
	        resetFunc();
	    }
	}

	/*
	 * @Desc 从目标字符串s中找到子串t 
	 */
	unsigned char findString(char *s, char *t, unsigned int Length_s, unsigned int Length_t)
	{   char x = 0; char *p; p = t;
	    int i = 0, j = 0;
	    for (; i < Length_s; s++, i++)
	    {
	        while (*t == *s)
	        {   s++; t++; i++; j++;
	            if (j >= Length_t) return 1;
	        }
	        s -= j;
	        t = p; j = 0;
	    }
	    return NULL;
	}


### 购买[*SENZ020 ESP8266 ESP-01S WiFi模块*](https://www.ebay.com/).