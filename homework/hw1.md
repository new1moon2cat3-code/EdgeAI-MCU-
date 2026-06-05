1.upload WebServer_ControlLED.ino to ChatGPT or Gemini
ask it to modify code to support two buttons to control LED_B and LED_G
submit smartphone screen that shows your phone connected to AMB82-mini running WebServer_ControlLEDx2

作業成果
![成果1](../images/hw1-1.png)
![成果2](../images/hw1-2.jpg)

程式碼:
``` 
#include <WiFi.h>


char ssid[] = "POCO F5";
char pass[] = "0983892962";


int status = WL_IDLE_STATUS;


WiFiServer server(80);


String ledBState = "off";
String ledGState = "off";


void setup() {
    Serial.begin(115200);


    pinMode(LED_B, OUTPUT);
    pinMode(LED_G, OUTPUT);


    while (status != WL_CONNECTED) {
        Serial.print("Connecting to ");
        Serial.println(ssid);


        status = WiFi.begin(ssid, pass);
        delay(10000);
    }


    server.begin();
    printWifiStatus();
}


void loop() {
    WiFiClient client = server.available();


    if (client) {
        Serial.println("new client");
        String currentLine = "";


        while (client.connected()) {
            if (client.available()) {
                char c = client.read();
                Serial.write(c);


                if (c == '\n') {


                    if (currentLine.length() == 0) {


                        client.println("HTTP/1.1 200 OK");
                        client.println("Content-type:text/html");
                        client.println();


                        client.println("<html>");
                        client.println("<head><meta name=\"viewport\" content=\"width=device-width, initial-scale=1\">");
                        client.println("<style>");
                        client.println("body{font-family:Helvetica;text-align:center;}");
                        client.println(".button{padding:16px 40px;font-size:25px;margin:10px;border-radius:12px;}");
                        client.println("</style></head>");


                        client.println("<body>");
                        client.println("
AMB82-mini LED Control
");


                        // LED_B
                        client.println("
LED_B : " + ledBState + "

");
                        client.println("<a href=\"/B_ON\"><button class=\"button\">LED_B ON");
                        client.println("<a href=\"/B_OFF\"><button class=\"button\">LED_B OFF");


                        // LED_G
                        client.println("
LED_G : " + ledGState + "

");
                        client.println("<a href=\"/G_ON\"><button class=\"button\">LED_G ON");
                        client.println("<a href=\"/G_OFF\"><button class=\"button\">LED_G OFF");


                        client.println("</body></html>");


                        client.println();
                        break;


                    } else {
                        currentLine = "";
                    }


                } else if (c != '\r') {
                    currentLine += c;
                }


                if (currentLine.endsWith("GET /B_ON")) {
                    digitalWrite(LED_B, HIGH);
                    ledBState = "on";
                }


                if (currentLine.endsWith("GET /B_OFF")) {
                    digitalWrite(LED_B, LOW);
                    ledBState = "off";
                }


                if (currentLine.endsWith("GET /G_ON")) {
                    digitalWrite(LED_G, HIGH);
                    ledGState = "on";
                }


                if (currentLine.endsWith("GET /G_OFF")) {
                    digitalWrite(LED_G, LOW);
                    ledGState = "off";
                }


            }
        }


        client.stop();
        Serial.println("client disconnected");
    }
}


void printWifiStatus() {


    Serial.print("SSID: ");
    Serial.println(WiFi.SSID());


    IPAddress ip = WiFi.localIP();
    Serial.print("IP Address: ");
    Serial.println(ip);


    Serial.print("Open browser: http://");
    Serial.println(ip);
}
``` 
