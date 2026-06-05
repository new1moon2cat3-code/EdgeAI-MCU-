作業內容
7.Example Codes
Examples > AmebaGPIO > DHT_Tester  (DHT11 on GPIO pin 8, reading Temperature & Humidity)
Examples > WiFi > SimpleHttpWeb > ReceiveData
Homework:
modify ReceiveData to show Temperature and Humidity Data by reading DHT11
上傳網頁截圖

成果
![成果1](../images/hw7.png)

程式碼:
``` 
#include <WiFi.h>
#include "DHT.h"

// ================= DHT11 =================
#define DHTPIN 7
#define DHTTYPE DHT11

DHT dht(DHTPIN, DHTTYPE);

// 儲存最新數值（重點）
float temperature = 0;
float humidity = 0;

// 控制讀取時間
unsigned long lastDHTRead = 0;

// ================= WiFi =================
char ssid[] = "POCO F5";
char pass[] = "0983892962";

WiFiServer server(80);

void setup()
{
    Serial.begin(115200);

    dht.begin();

    Serial.println("Connecting WiFi...");

    while (WiFi.begin(ssid, pass) != WL_CONNECTED)
    {
        delay(2000);
        Serial.println("Connecting...");
    }

    Serial.println("WiFi Connected!");
    Serial.print("IP: ");
    Serial.println(WiFi.localIP());

    server.begin();
}

void loop()
{
    // =======================
    // 1. 每2秒讀一次 DHT11
    // =======================
    if (millis() - lastDHTRead > 2000)
    {
        float h = dht.readHumidity();
        float t = dht.readTemperature();

        if (!isnan(h) && !isnan(t))
        {
            humidity = h;
            temperature = t;

            Serial.print("T:");
            Serial.print(temperature);
            Serial.print(" C  H:");
            Serial.print(humidity);
            Serial.println(" %");
        }
        else
        {
            Serial.println("DHT read failed (ignored)");
        }

        lastDHTRead = millis();
    }

    // =======================
    // 2. HTTP Server
    // =======================
    WiFiClient client = server.available();

    if (client)
    {
        Serial.println("Client connected");

        String reqLine = "";

        while (client.connected())
        {
            if (client.available())
            {
                char c = client.read();

                if (c == '\n')
                {
                    if (reqLine.length() == 0)
                    {
                        // ========== HTML ==========
                        String html = "";

                        html += "<!DOCTYPE html>";
                        html += "<html>";
                        html += "<head>";
                        html += "<meta charset='utf-8'>";
                        html += "<meta http-equiv='refresh' content='3'>";
                        html += "<title>DHT11</title>";
                        html += "</head>";

                        html += "<body style='text-align:center;font-family:Arial'>";

                        html += "<h1>DHT11 Sensor</h1>";

                        html += "<h2 style='color:red'>Temperature: ";
                        html += String(temperature);
                        html += " °C</h2>";

                        html += "<h2 style='color:blue'>Humidity: ";
                        html += String(humidity);
                        html += " %</h2>";

                        html += "</body></html>";

                        // ========== HTTP response ==========
                        client.println("HTTP/1.1 200 OK");
                        client.println("Content-Type: text/html");
                        client.println("Connection: close");
                        client.println();
                        client.println(html);

                        break;
                    }
                    else
                    {
                        reqLine = "";
                    }
                }
                else if (c != '\r')
                {
                    reqLine += c;
                }
            }
        }

        delay(1);
        client.stop();
``` 
        Serial.println("Client disconnected");
    }
}
