//ekalaivan28@gmail.com
//BLYNKIOT

#include <Servo.h>
#define BLYNK_TEMPLATE_ID "TMPL3OXc2Pn1K"
#define BLYNK_TEMPLATE_NAME "ARMY"
#define BLYNK_AUTH_TOKEN "LeIowxs6GlqZruS6Kxa7pg4BLUtCI6uJ"


/* Comment this out to disable prints and save space */
#define BLYNK_PRINT Serial


#include <ESP8266WiFi.h>
#include <BlynkSimpleEsp8266.h>
Servo gateServo;  // create servo object to control a servo

//
// Your WiFi credentials.
// Set password to "" for open networks.
char ssid[] = "ROG5";
char pass[] = "12341234";

#define VPIN_1    V0 
#define VPIN_2    V1 
#define VPIN_3    V2  // Theft
#define VPIN_4    V3  // Lt
#define VPIN_5    V4  // Long
BLYNK_CONNECTED()
{ 
  Blynk.syncVirtual(V0);  
  Blynk.syncVirtual(V1);  
  Blynk.syncVirtual(V2);  
  Blynk.syncVirtual(V3);  
  Blynk.syncVirtual(V4);   
}
void setup()
{
  // Debug console
  Serial.begin(9600);
  delay(100);
  Blynk.begin(BLYNK_AUTH_TOKEN, ssid, pass);
  //pinMode(D1,OUTPUT); 
  gateServo.attach(D3);  // att
  Blynk.virtualWrite(VPIN_1,LOW); 
  Blynk.virtualWrite(VPIN_2,LOW); 
  Blynk.virtualWrite(VPIN_3,LOW); 
  Blynk.virtualWrite(VPIN_4,LOW); 
  
}
BLYNK_WRITE(V4) //up
{
  // Set incoming value from pin V0 to a variable
  int value = param.asInt();
  if (value == 1)
  {
    // digitalWrite(D1,LOW);
    openGate();
  delay(500);  // Wait for 5 seconds
  closeGate();
  delay(500);  //
     Serial.print('t');
     delay(100);
  }
  else
  {
     //digitalWrite(D1,LOW);
     closeGate();
    delay(500);  // Wait for 5 seconds
     Serial.print('T');
     delay(100);    
    }
  
}
void loop()
{
  Blynk.run();
  if (Serial.available()>0) 
  {
  char a=Serial.read();
  //Serial.print(a);
if (a=='A') // Accident
{
  Blynk.virtualWrite(VPIN_1,HIGH); 
}
else if (a=='B')
{
  Blynk.virtualWrite(VPIN_2,HIGH); 
  }

if (a=='C') // Theft 
{
  Blynk.virtualWrite(VPIN_3,HIGH); 
}
else if (a=='D')
{
  Blynk.virtualWrite(VPIN_4,HIGH); 
  }  
  }  
}
void openGate() {
  gateServo.write(180);  // move the servo to 180 degrees (open position)
}

void closeGate() {
  gateServo.write(0);  // move the servo to 0 degrees (closed position)
}
