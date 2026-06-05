作業內容
5.Examples:
AmebaWire > VL53L0X > Continuous
AmebaSPI > LCD_Screen_ILI9341_TFT
Homework:
use I2C1 bus (SDA1, SCL1) by setting wire1.begin()
and modify VL53L0X.cpp in Realtek hardware package library
(C:\Users\User\AppData\Local\Arduino15\packages\realtek\hardware\AmebaPro2\4.1.1-build20260417\libraries\Wire\src\VL53L0X_IR_libraries/VL53L0X.cpp)
VL53L0X::VL53L0X():
    bus(&Wire1),
sample:
#include <Wire.h>
#include <VL53L0X_IR_libraries/VL53L0X.h>
#include "SPI.h"
#include "AmebaILI9341.h"


#define TFT_RESET 5
#define TFT_DC    4
#define TFT_CS    SPI_SS
#define ILI9341_SPI_FREQUENCY 20000000


VL53L0X sensor;
AmebaILI9341 tft = AmebaILI9341(TFT_CS, TFT_DC, TFT_RESET);
void setup()
{
    Serial.begin(115200);
    Wire1.begin();


...

請繳交照片顯示紅外線感測+TFT顯示距離(cm)


成果
![成果1](../images/hw5-1.jpg)
![成果2](../images/hw5-2.jpg)

程式碼:
... 
#include <Wire.h>
#include <VL53L0X_IR_libraries/VL53L0X.h>


#include "SPI.h"
#include "AmebaILI9341.h"


// TFT LCD 腳位設定
#define TFT_RESET 5
#define TFT_DC    4
#define TFT_CS    SPI_SS


#define ILI9341_SPI_FREQUENCY 20000000


// 建立 TFT 與 VL53L0X 物件
AmebaILI9341 tft = AmebaILI9341(TFT_CS, TFT_DC, TFT_RESET);
VL53L0X sensor;


void setup()
{
    Serial.begin(115200);


    // 使用 I2C1
    Wire1.begin();


    // SPI TFT 初始化
    SPI.setDefaultFrequency(ILI9341_SPI_FREQUENCY);


    tft.begin();


    tft.setRotation(1);


    // 清空畫面
    tft.fillScreen(ILI9341_BLACK);


    // 顯示啟動畫面
    tft.setCursor(20, 40);


    tft.setForeground(ILI9341_GREEN);


    tft.setFontSize(3);


    tft.println("VL53L0X");


    tft.setCursor(20, 90);


    tft.setFontSize(2);


    tft.println("Initializing...");


    // VL53L0X 初始化
    sensor.setTimeout(500);


    if (!sensor.init())
    {
        Serial.println("VL53L0X Failed");


        tft.fillScreen(ILI9341_BLACK);


        tft.setCursor(20, 60);


        tft.setForeground(ILI9341_RED);


        tft.setFontSize(3);


        tft.println("Sensor Fail");


        while (1);
    }


    // 啟動連續測距
    sensor.startContinuous();


    delay(1000);
}


void loop()
{
    // 讀取距離(mm)
    int distance = sensor.readRangeContinuousMillimeters();


    // 轉換成 cm
    float cm = distance / 10.0;


    // Serial Monitor 顯示
    Serial.print("Distance: ");


    Serial.print(cm);


    Serial.println(" cm");


    // TFT 顯示
    tft.fillScreen(ILI9341_BLACK);


    // 標題
    tft.setCursor(20, 30);


    tft.setForeground(ILI9341_CYAN);


    tft.setFontSize(3);


    tft.println("Distance");


    // 距離數值
    tft.setCursor(20, 100);


    tft.setForeground(ILI9341_YELLOW);


    tft.setFontSize(5);


    tft.print(cm);


    tft.println(" cm");


    delay(200);
}
... 
