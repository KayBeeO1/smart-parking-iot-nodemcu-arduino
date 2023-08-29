#define BLYNK_TEMPLATE_ID "TMPLWmtBLBGt"
#define BLYNK_DEVICE_NAME "Smart Parking"
#define BLYNK_AUTH_TOKEN "EtGkyg0wr5MIwqhaLfYxwvnoX22dGm6s"
#define BLYNK_PRINT Serial
#include <ESP8266WiFi.h>
#include <BlynkSimpleEsp8266.h>
#define  led_1  D4
#define  trig1  D1
#define  echo1  D2
#define  trig2  D3
#define  echo2  D0
#define  trig3  D5
#define  echo3  D6
#define  trig4  D7
#define  echo4  D8

long  duration1;
long  duration2;
long  duration3;
long  duration4;
int  distance1;
int  distance2;
int  distance3;
int  distance4;
char auth[] = BLYNK_AUTH_TOKEN;
char ssid[] = "FTTH";
char pass[] = "11gao541";

BlynkTimer timer;
WidgetLCD lcd(V20);
WidgetLED led1(V16);
WidgetLED led2(V17);
WidgetLED led3(V18);
WidgetLED led4(V19);

void setup()
{
  pinMode(trig1,OUTPUT);  
  pinMode(echo1,INPUT);   
  pinMode(trig2,OUTPUT);  
  pinMode(echo2,INPUT);   
  pinMode(trig3,OUTPUT);
  pinMode(echo3,INPUT);
  pinMode(trig4,OUTPUT);  
  pinMode(echo4,INPUT);
  pinMode(led_1,OUTPUT);
  Serial.begin(9600);
  Blynk.begin(auth, ssid,pass);
  timer.setInterval(1000L,sendSensor);

}

void loop()
{
  Blynk.run();
  timer.run();
}
void sendSensor()
{ 
  digitalWrite(trig1,LOW);   // Makes trig1 Pin low
  delayMicroseconds(2);       // 2 micro second delay
 
  digitalWrite(trig1,HIGH);  // trig1 Pin high
  delayMicroseconds(10);      // trig1 Pin high for 10 micro seconds
  digitalWrite(trig1,LOW);   // trig1 Pin low

  duration1 = pulseIn(echo1,HIGH);   //Read echo1 pin, time in microseconds
  distance1 = duration1*0.034/2;   //Calculating actual/real distance
  Serial.print(" Distance 1 : ");
  Serial.println(distance1);

  if(distance1 <= 8)
  {
    led1.on();
  }
  else
  {
    led1.off(); 
  } 

  digitalWrite(trig2,LOW);   // Makes trig2 Pin low
  delayMicroseconds(2);       // 2 micro second delay
  
  digitalWrite(trig2,HIGH);  // trig2 Pin high
  delayMicroseconds(10);      // trig2 Pin high for 10 micro seconds
  digitalWrite(trig2,LOW);   // trig2 Pin low  
  duration2 = pulseIn(echo2,HIGH);   //Read echo2 pin, time in microseconds
  distance2 = duration2*0.034/2;   //Calculating actual/real distance
  Serial.print(" Distance 2 : ");
  Serial.println(distance2);
  
  if(distance2 <= 8)
  {
    led2.on();
  }
  else
  {
    led2.off();
  }   

  digitalWrite(trig3,LOW);   // Makes trig3 Pin low
  delayMicroseconds(2);       // 2 micro second delay
 
  digitalWrite(trig3,HIGH);  // trig3 Pin high
  delayMicroseconds(10);      // trig3 Pin high for 10 micro seconds
  digitalWrite(trig3,LOW);   // trig3 Pin low

  duration3 = pulseIn(echo3,HIGH);   //Read echo3 pin, time in microseconds
  distance3 = duration3*0.034/2;   //Calculating actual/real distance
  Serial.print(" Distance 3 : ");
  Serial.println(distance3);
  
  if(distance3 <= 8)
  {
    led3.on();
  }
  else
  {
    led3.off(); 
  }

  digitalWrite(trig4,LOW);   // Makes trig4 Pin low
  delayMicroseconds(2);       // 2 micro second delay
 
  digitalWrite(trig4,HIGH);  // trig4 Pin high
  delayMicroseconds(10);      // trig4 Pin high for 10 micro seconds
  digitalWrite(trig4, LOW);   // trig4 Pin low

  duration4=pulseIn(echo4, HIGH);   //Read echo4 pin, time in microseconds
  distance4=duration4*0.034/2;   //Calculating actual/real distance
  Serial.print(" Distance 4 : ");
  Serial.println(distance4);


  if(distance4 <= 8)
  {
    led4.on();
  }
  else
  {
    led4.off();   
  }
  
  if((distance1 <=8)&&(distance2 <=8)&&(distance3 <=8)&&(distance4 <=8))
  {
    lcd.clear();
    digitalWrite(led_1, HIGH);
    lcd.print(0, 0, "   Parking lot  ");
    lcd.print(0, 1, "       Full ");
    delay(1000);
  }
  else
  { lcd.clear();
    digitalWrite(led_1, LOW);
    lcd.print(0, 0, "   Parking lot  ");
    lcd.print(0, 1, "    Available ");
    delay(1000);  
  }
}
