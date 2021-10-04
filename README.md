echo "# GAS-SENSOR-MQ135" >> README.md

https://github.com/DINESHKP08/GAS-SENSOR-MQ135.git

#define BLYNK_PRINT Serial
#include <WiFi.h>
#include <WiFiClient.h>
#include <BlynkSimpleEsp32.h>
BlynkTimer timer;
 
// You should get Auth Token in the Blynk App.
// Go to the Project Settings (nut icon).
char auth[] = "YourAuthToken";
 
// Your WiFi credentials.
// Set password to "" for open networks.
char ssid[] = "YourNetworkName";
char pass[] = "YourPassword";
 
#define MQ135 34
#define GREEN 16
#define RED 17
int sensorValue = 0;
boolean state = false;
 
void setup()
{
 // Debug console
 Serial.begin(115200);
 Blynk.begin(auth, ssid, pass);
 pinMode(MQ135, INPUT);
 pinMode(GREEN, OUTPUT);
 pinMode(RED, OUTPUT);
 timer.setInterval(1000L, sendUptime);
}
 
void sendUptime()
{
 
 sensorValue = analogRead(MQ135);
 Blynk.virtualWrite(V1, sensorValue);
 Serial.println(sensorValue);
 
 if (sensorValue > 600)
 {
 Blynk.notify("Gas Detected!");
 digitalWrite(GREEN, LOW);
 digitalWrite(RED, HIGH);
 }
 
 else
 {
 digitalWrite(GREEN, HIGH);
 digitalWrite(RED, LOW);
 }
}
 
void loop()
{
 Blynk.run();
 timer.run();
}
