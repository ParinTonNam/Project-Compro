# IOT แจ้งเตือนการเปิดปิดประตู
<!-- <img src="./img/ProjectCompro_1.png" width=600 height=750> -->
![ProjectCompro_1](img/ProjectCompro_1.png)

# บทคัดย่อ 
ชื่อวิจัยภาษาไทย :  เซนเซอร์แจ้งเตือนผ่านไลน์ \
ชื่อวิจัยภาษาอังกฤษ : Motion Sensor with App Notification \
ชื่อผู้วิจัย : \
&emsp;นาย เกียรกำธร พูลพล 64070009 \
&emsp;นาย ปรินทร์ เดชากรณ์ 64070064 \
&emsp;นาย อนุวัฒน์ ประสิทธ์ 64070115 \
&emsp;นาย ศุภกร ดาราสุนทโร 64070238 \
ปีที่ทำการวิจัย : 2565 \
การวิจัยครั้งนี้มีวัตถุประสงค์ 
- เพื่อสร้างระบบที่อำนวยความสะดวกให้ผู้ใช้งานได้ทั้ง ในอาคารบ้านเรือน หรือสถานที่ ต่างๆ 
- ศึกษาการแจ้งเตือนจากอุปกรณ์เทคโนโลยีของ Line เพื่ออำนวยความสะดวกแก่ผู้ใช้งาน 
- ใช้เป็นกรณีศึกษาในการทำวิจัยในอนาคต 

สรุปผลการวิจัย
- จากการทดลองการทำงานของเซนเซอร์แจ้งเตือนมีการทำงานได้ตามปกติ แต่จะมีปัญหา เมื่อผู้ใช้อยู่ในสถานที่ที่ไม่มีบริการสัญญาณ Internet ทำให้เกิดการเชื่อมต่อไม่ติด

ข้อเสนอแนะ :\
คำสำคัญ : เซนเซอร์ , เทคโนโลยี, อำนวยความสะดวก

# อุปกรณ์
1.IR Infrared Obstacle Avoidance Sensor Module\
2.หัวชาร์จ USB\
3.สาย Micro USB \
4.NodeMcu ESP8266\

# Code
การเรียกใช้ฟังก์ชัน\
```cpp
#include <ESP8266WiFi.h>
#include <DHT.h>
```
การกำหนดตัวแปร\
```cpp
#define WIFI_SSID "ชื่อ wifi" //เปลี่ยนเป็นของตนเอง
#define WIFI_PASSWORD "รหัส wifi"//เปลี่ยนเป็นของตนเอง
#define LINE_TOKEN_PIR "line token"//เปลี่ยนเป็นของตนเอง
#define PirPin D6
#define DHTPIN D7
#define DHTTYPE           DHT11
```
รับผลจาก Input\
```cpp
DHT dht(DHTPIN, DHTTYPE);
String message1 = "ข้อความแจ้งเตือน";//เปลี่ยนเป็นของตนเอง
bool beep_state = false;
bool send_state = false;
uint32_t ts, ts1, ts2;
```
การ setup ตั้งค่าส่วนต่างๆ(LED, Pin, WIFI)\
```cpp
void setup() {
 Serial.begin(115200);
 Serial.println();
 pinMode(PirPin, INPUT);
 pinMode(LED_BUILTIN, OUTPUT);
 digitalWrite(LED_BUILTIN, HIGH);
 dht.begin();
 Serial.println("connecting");
 WiFi.mode(WIFI_STA);
 WiFi.begin(WIFI_SSID, WIFI_PASSWORD);
 Serial.print("connecting");
 while (WiFi.status() != WL_CONNECTED) {
   Serial.print(".");
   delay(500);
 }
 Serial.println();
 Serial.print("connected: ");
 Serial.println(WiFi.localIP());
 delay(10000);
 Serial.println("Pir Ready!!");
 read_sensor();
 ts = ts1 = ts2 = millis();
}
```
