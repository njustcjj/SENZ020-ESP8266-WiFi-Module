# SENZ020-ESP8266-ESP-01S-WiFi-Module

###### Translation

> For `English`, please click [`here.`](https://github.com/njustcjj/SENZ020-ESP8266-WiFi-Module/blob/master/README.md)

> For `Chinese`, please click [`here.`](https://github.com/njustcjj/SENZ020-ESP8266-WiFi-Module/blob/master/README_CN.md)

![](https://github.com/njustcjj/SENZ020-ESP8266-WiFi-Module/blob/master/pic/SENZ020.jpg "SENZ020")


### Introduction


> SENZ020 ESP8266 ESP-01S Moudle has a powerful on-chip processing and storage capacity, built-in 32-bit processor, built-in Lwip protocol stack.
> And SENZ020 is a Serial-to-WIFI module applicable to a variety of 3.3V single-chip system. It can be used for Arduino, wireless data transfer, remote control.  It supports AP+STA mode co-exist. And you could configure various parameters via AT commands.

### Specification

- Working voltage: +3.3V
- Wireless standard: IEEE802.11b/g/n
- Frequency: 2.4 GHz
- Working mode: STA/ soft-AP/ soft-AP + STA
- Support WEP/WPA-PSK/WPA2-PSK encryption
- Support UART, TTL, GPIO interface
- Built-in Lwip protocol stack
- Built-in low-power 32-bit CPU: can work as an application processor
- Size：24.7 x 14.4mm


### Tutorial

#### Pin Definition

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


![](https://github.com/njustcjj/SENZ020-ESP8266-WiFi-Module/blob/master/pic/SENZ020_pin.jpg "Pin Definition") 

#### Connecting Diagram

![](https://github.com/njustcjj/SENZ020-ESP8266-WiFi-Module/blob/master/pic/SENZ020_connect.PNG "Connecting Diagram") 


### Sample Code

#### Sample1:

> To develop with serial AT commands.  
> Download description for [`AT_Command_Doc`](http://github.com/njustcjj/SENZ020-ESP8266-WiFi-Module/blob/trunk/doc/AT_Command_Doc.pdf)

	/*
	RXD <-> Pin3, TXD <-> Pin2;
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
	#define SSID "xxxxxxx"        //wifi name
	#define PASSWORD "xxxxxxx"        //wifi password
	#define LED 13 //状态指示灯

	WIFI wifi;

	/*
	 * @Desc initialization
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
	 *
	 * Step1：Set serial port
	 * Step2: set ESP8266
	 *    1) AT+RST restart the mode
	 *    2) AT+CWMODE=1 and AT+RST  Set STA mode and restart to vaildate 
	 *    3) AT+CWJAP="SSID","PASSWORD"  set WiFi name and password
	 *    4) AT+CIPMUX=0    0 single route connect mode, 1 multi route connec mode
	 *    5) AT+CIFSR to get IP and MAC address
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

	void(* resetFunc) (void) = 0; //Reset command

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
	 * find char t from tagert char s
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


### Purchasing [*SENZ020-ESP8266-ESP-01S-WiFi-Module*](https://www.ebay.com/).
