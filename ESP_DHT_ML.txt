/*----------------------------------------Libraries-------------------------------------------------*/
#include <ESP8266WiFi.h>
#include <WiFiClientSecure.h>
#include "DHTesp.h"

DHTesp dht;
WiFiClientSecure client;

const char* ssid     = "Papaiya's";
const char* password = "DHIAPRARNY";
const char* host = "script.google.com";
const char* fingerprint = "e7 41 23 de 74 00 6d 9b 0a fd 39 46 e9 49 59 0a 36 f2 cc 2a";
String url;
const int httpsPort = 443;

void setup()
{
  Serial.begin(115200);
  Serial.println();
  Serial.println();
  Serial.print("Connecting to ");
  Serial.println(ssid);

  WiFi.mode(WIFI_STA);
  WiFi.begin(ssid, password); 
  while (WiFi.status() != WL_CONNECTED) 
  {
    delay(500);
    Serial.print(".");
  }
 
  Serial.println("");
  Serial.println("WiFi connected");  
  Serial.println("IP address: ");
  Serial.println(WiFi.localIP());
  Serial.print("Netmask: ");
  Serial.println(WiFi.subnetMask());
  Serial.print("Gateway: ");
  Serial.println(WiFi.gatewayIP());
  client.setInsecure();

  /*Connect DHT11 sensor to GPIO 16 = D0*/ 
  dht.setup(16, DHTesp::DHT11); 
}

void loop()
{
  /*---------------------------Establish connection with Host using https protocol------------------------------------*/
  Serial.print("connecting to ");
  Serial.println(host);
 
  if (!client.connect(host, httpsPort)) 
  {
    Serial.println("connection failed");
    return;
  }

  if (client.verify(fingerprint, host)) 
  {
    Serial.println("certificate matches");
  } 
  else 
  {
    Serial.println("certificate doesn't match");
  }
  
  /*------------------------------------------Temperature Measurement-------------------------------------------------*/
  delay(dht.getMinimumSamplingPeriod());
  float temperature = dht.getTemperature();

  Serial.print("Status: ");
  Serial.print(dht.getStatusString());
  Serial.print("\tTemperature in celsius = ");
  Serial.println(temperature, 1);

  /*----------------------------------------Establish connection with URL----------------------------------------------*/
  url = "/macros/s/AKfycbxghA38mhEAce3L_OIeYy4eEZdDm8u5rO1L1EmUqxTsHaybAc4/exec?func=addData&val="+ String(temperature);
  Serial.print("Requesting URL: ");
  Serial.println(url);
  
  client.print(String("GET ") + url + " HTTP/1.1\r\n" +
               "Host: " + host + "\r\n" + 
               "Connection: close\r\n\r\n");
  delay(500);
  String section="header";
  while(client.available()) 
  {
    String line = client.readStringUntil('\r');
    Serial.print(line);
  }
  Serial.println();
  Serial.println("closing connection");
  
  /*Delay of 5 seconds.*/
  delay(5000);
}
