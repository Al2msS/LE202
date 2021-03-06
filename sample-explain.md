# Sample-Explain
## Program 01 Serial-Monitor
เป็นโปรแกรมที่นับเลขไปเรื่อยๆทุก1000ms
ในส่วน setup จะเป็นการตั้งค่าให้บอร์ดและคอมพิวเตอร์ใช้ความเร็วเดียวกันในการสื่อสารอย่างในตัวอย่าโค๊ดจะใช้ความเร็ว 115,200 บิตต่อวินาที
และในส่วน loop จะทำการแสดงผลนับตัวเลขไปเรื่อยทุกๆ 1000ms
### Code 
	#include <Arduino.h>

	int cnt = 0;

	void setup()
	{
		Serial.begin(115200);
	}

	void loop()
	{
		cnt++;
		Serial.printf("PATTANI :%d\n",cnt);
		int s = cnt % 5 + 1;
		int d = s * 1000;
		delay(d);
	}

## Program 02 Scan-Wifi
จะเป็นโปรแกรมในการค้นหาwifiรอบๆตัว
ในส่วน setup จะเป็นการตั้งค่าให้บอร์ดและคอมพิวเตอร์ใช้ความเร็วเดียวกันในการสื่อสารและตั้งค่า wifi และยกเลิกการเชื่อมต่อของ wifi ก่อนหน้า
และในส่วน loo จะเป็นการค้นหา wifi พร้อมแสดง wifi ที่มีอยู่รอบๆตัวในขณะนั้น
### Code 
	#include <Arduino.h>
	#include <ESP8266WiFi.h>

	int cnt = 0;

	void setup()
	{
		Serial.begin(115200);
		WiFi.mode(WIFI_STA);
		WiFi.disconnect();
		delay(100);
		Serial.println("\n\n\n");
	}

	void loop()
	{
		Serial.println("========== เริ่มต้นแสกนหา Wifi ===========");
		int n = WiFi.scanNetworks();
		if(n == 0) {
			Serial.println("NO NETWORK FOUND");
		} else {
			for(int i=0; i<n; i++) {
				Serial.print(i + 1);
				Serial.print(": ");
				Serial.print(WiFi.SSID(i));
				Serial.print(" (");
				Serial.print(WiFi.RSSI(i));
				Serial.println(")");
				Serial.print(WiFi.channel(i));
				delay(10);
			}
		}
		Serial.println("\n\n");
		delay(10 * 1000);
	}
## Program 03 Output-port
เป็นโปรแกรมที่ส่งสัญญาณดิจิทัลออกมาที่outputที่กำหนด
ในส่วน setup จะเป็นการตั้งค่าให้บอร์ดและคอมพิวเตอร์ใช้ความเร็วเดียวกันในการสื่อสารและตั้งค่าให้ pin0 เป็น output
และในส่วน loop จะทำการนับcntทุกๆ 500ms และจะแสดงผล ON พร้อมส่งสัญญาน 1 ออกมาเมื่อ cnt เป็นคู่และจะแสดงผลเป็น OFF พร้อมส่งสัญญาน 0 ออกมาเมื่อcntเป็นคี่
### Code 
	#include <Arduino.h>
	#include <ESP8266WiFi.h>

	int cnt = 0;

	void setup()
	{
		Serial.begin(115200);
		pinMode(0, OUTPUT);
		Serial.println("\n\n\n");
	}

	void loop()
	{
		cnt++;
		if(cnt % 2) {
			Serial.println("========== ON ===========");
			digitalWrite(0, HIGH);
		} else {
			Serial.println("========== OFF ===========");
			digitalWrite(0, LOW);
		}
		delay(500);
	}

## Program 04
จะเป็นโปรแกรมที่รับสัญญาณมาจากinputและส่งสัญญาณออกมาที่output
ในส่วน setup จะเป็นการตั้งค่าให้บอร์ดและคอมพิวเตอร์ใช้ความเร็วเดียวกันในการสื่อสารและตั้งค่าให้ pin0 เป็น input และตั้ง pin2 เป็น output
และในส่วน loop จะรับสัญญาณจาก pin0 มาและทำการตรวจสอบถ้าสัญญาณที่ได้มีค่าเป็น 1 ให้ส่งสัญญาณ 0 ออกไปที่ pin2 และถ้าหากสัญญาณที่ได้มีค่าเป็น 0 ให้ส่งสัญญาณ 1 ออกไปที่ pin2
### Code 
	#include <Arduino.h>
	#include <ESP8266WiFi.h>

	int cnt = 0;

	void setup()
	{
		Serial.begin(115200);
		pinMode(0, INPUT);
		pinMode(2, OUTPUT);
		Serial.println("\n\n\n");
	}

	void loop()
	{
		int val = digitalRead(0);
		Serial.printf("======= read %d\n", val);
		if(val==1) {
			digitalWrite(2, LOW);
		} else {
			digitalWrite(2, HIGH);
		}
		delay(100);
	}
## Program 05 Wifi
เป็นโรแกรมในการเชื่อมต่อไวไฟที่มีอยู่แล้ว
ในส่วน setup จะเป็นการเชื่อมต่อ wifi และทำการสร้างweb sever 
และในส่วน loop จะเป็นตรวจสอบว่ามีคนมาเชื่อมต่อก้บเซิฟเวอร์ของเรารึยัง
### Code 
	#include <ESP8266WiFi.h>
	//#include <WiFiClient.h>
	#include <ESP8266WebServer.h>

	const char* ssid = "HI_BMFWIFI_2.4G";
	const char* password = "0819110933";

	ESP8266WebServer server(80);

	int cnt = 0;

	void setup(void){
		Serial.begin(115200);

		WiFi.mode(WIFI_STA);
		WiFi.begin(ssid, password);
		while (WiFi.status() != WL_CONNECTED) {
			delay(500);
			Serial.print(".");
		}
		Serial.print("\n\nIP address: ");
		Serial.println(WiFi.localIP());

		server.onNotFound([]() {
			server.send(404, "text/plain", "Path Not Found");
		});

		/// http://192.0.0.1/ = Hello cnt: ???
		server.on("/", []() {
			cnt++;
			String msg = "Hello cnt: ";
			msg += cnt;
			server.send(200, "text/plain", msg);
		});
		/// http://192.0.0.1/on = Hello cnt: ???
		server.on("/on", []() {
			cnt++;
			String msg = "Switch on ";
			msg += cnt;
			server.send(200, "text/plain", msg);
		});
		/// http://192.0.0.1/off = Switch off: ???
		server.on("/off", []() {
			cnt++;
			String msg = "Hello cnt: ";
			msg += cnt;
			server.send(200, "text/plain", msg);
		});

		server.begin();
		Serial.println("HTTP server started");
	}

	void loop(void){
	  server.handleClient();
	}

## Program 06 Wifi AP
จะเป็นการสร้าง Acesspoint wifi
ในส่วน setup จะเป็นการกำหนดชื่อ wifi ip gatway subnet ของตัวไวไฟ
และในส่วน loop จะเป็นจะเป็นตรวจสอบว่ามีคนมาเชื่อมต่อก้บเซิฟเวอร์ของเรารึยัง
### Code 
	#include <ESP8266WiFi.h>
	//#include <WiFiClient.h>
	#include <ESP8266WebServer.h>

	const char* ssid = "MY-ESP8266";
	const char* password = "choompol";

	IPAddress local_ip(192, 168, 1, 1);
	IPAddress gateway(192, 168, 1, 1);
	IPAddress subnet(255, 255, 255, 0);

	ESP8266WebServer server(80);

	int cnt = 0;

	void setup(void){
		Serial.begin(115200);

		WiFi.softAP(ssid, password);
		WiFi.softAPConfig(local_ip, gateway, subnet);
		delay(100);

		server.onNotFound([]() {
			server.send(404, "text/plain", "Path Not Found");
		});

		server.on("/", []() {
			cnt++;
			String msg = "Hello cnt: ";
			msg += cnt;
			server.send(200, "text/plain", msg);
		});

		server.begin();
		Serial.println("HTTP server started");
	}

	void loop(void){
	  server.handleClient();
	}
