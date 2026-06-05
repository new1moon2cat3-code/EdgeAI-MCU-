作業內容
8.Code Examples:
Examples > AmebaSPI > Camera_2_Lcd_JPEGDEC (Camera to LCD with JPEG decoder)
Examples > AmebaNN > MultimediaAI > GenAIVisionTTS (Camera to LLM + TTS)
Applications:
1. AI 輔助回收物分類系統 
String prompt_msg = "請問這個回收物是什麼?請用中文回答";
2. AI輔助英語讀字卡造句
String prompt_msg_img = "Just say the word in the picture?";
3. AI看圖說故事
String prompt_msg = "看這張圖 請用中文給我一個簡短的故事";
4. AI情緒感知音樂播放器
String prompt_msg = "Analyze the emotion (happy, angry, sad, or joyful) of the person in the image. Based on the detected emotion, recommend a suitable song filename from the following list. Respond in the exact format: 'Emotion: [emotion], Song: [filename.mp3]'. Emotion mapping: Happy: APT.mp3. Angry: BirdsOfAFeather.mp3. Sad: ThePowerOfGoodBye.mp3. Joyful: AstroBunny.mp3. Other songs available: gTTS.mp3, IBelieve.mp3, JarOfLove.mp3, LoversMisses.mp3, Stumblin_In.mp3, YUNGBLUD.mp3.";
Homework
1) 實作以上任一個應用(或自創應用)，功能包含拍照+AI Vision + TTS + LCD顯示圖片或文字
2) 上傳成果影片及程式碼

程式碼:
``` 
String Gemini_key = "AIzaSyCepU10xC7_khWfh0DWKKJbq4SAVzKnWQA";


char wifi_ssid[] = "POCO F5";
char wifi_pass[] = "0983892962";


#include <WiFi.h>
#include "GenAI.h"
#include "VideoStream.h"
#include "SPI.h"
#include "AmebaILI9341.h"
#include "AmebaFatFS.h"


WiFiSSLClient client;


GenAI llm;
GenAI tts;


AmebaFatFS fs;


String mp3Filename = "story.mp3";


// Camera setting
VideoSetting config(768, 768, CAM_FPS, VIDEO_JPEG, 1);


#define CHANNEL 0


uint32_t img_addr = 0;
uint32_t img_len = 0;


// Button
const int buttonPin = 1;


// AI Prompt
String prompt_msg =
"看這張圖，請用中文講一個20字以內的簡短故事，只輸出一句話";


// TFT pins
#define TFT_RESET 5
#define TFT_DC    4
#define TFT_CS    SPI_SS


#define ILI9341_SPI_FREQUENCY 20000000


AmebaILI9341 tft = AmebaILI9341(TFT_CS, TFT_DC, TFT_RESET);


// =========================
// WiFi
// =========================
void initWiFi()
{
    tft.println("Connecting WiFi...");


    WiFi.begin(wifi_ssid, wifi_pass);


    while (WiFi.status() != WL_CONNECTED) {
        delay(500);
        tft.print(".");
    }


    tft.println("");
    tft.println("WiFi Connected!");
}


// =========================
// TFT
// =========================
void init_tft()
{
    tft.begin();


    tft.setRotation(2);


    tft.clr();


    tft.setCursor(0, 0);


    tft.setForeground(ILI9341_GREEN);


    tft.setFontSize(2);
}


// =========================
// Setup
// =========================
void setup()
{
    Serial.begin(115200);


    SPI.setDefaultFrequency(ILI9341_SPI_FREQUENCY);


    // TFT first
    init_tft();


    // WiFi
    initWiFi();


    // Camera
    config.setRotation(0);


    Camera.configVideoChannel(CHANNEL, config);


    Camera.videoInit();


    Camera.channelBegin(CHANNEL);


    // IO
    pinMode(buttonPin, INPUT);


    pinMode(LED_B, OUTPUT);


    tft.println("");
    tft.println("AI Story Ready");
}


// =========================
// Loop
// =========================
void loop()
{
    tft.setCursor(0, 80);


    tft.println("Press Button");


    if (digitalRead(buttonPin) == 1) {


        // LED flash
        for (int i = 0; i < 3; i++) {


            digitalWrite(LED_B, HIGH);


            delay(200);


            digitalWrite(LED_B, LOW);


            delay(200);
        }


        // Clear LCD
        tft.clr();


        tft.setCursor(0, 0);


        tft.println("Capturing...");
       
        // Take photo
        Camera.getImage(CHANNEL, &img_addr, &img_len);


        delay(500);


        // AI analyzing
        tft.println("");
        tft.println("AI Thinking...");


        // Gemini Vision
        String story = llm.geminivision(
            Gemini_key,
            "gemini-2.0-flash",
            prompt_msg,
            img_addr,
            img_len,
            client
        );


        // 防呆
        if (story.indexOf("quota") != -1 ||
            story.indexOf("Quota") != -1 ||
            story.indexOf("error") != -1 ||
            story.length() < 5) {


            story = "這是一個普通的日常場景。";
        }


        Serial.println(story);


        // LCD show result
        tft.clr();


        tft.setCursor(0, 0);


        tft.println("AI Story:");


        tft.println("");


        tft.println(story);


        // TTS
        tts.googletts(
            mp3Filename,
            story,
            "zh-TW"
        );


        delay(500);


        // Play MP3
        sdPlayMP3(mp3Filename);


        delay(3000);


        tft.clr();
    }
}


// =========================
// MP3 Play
// =========================
void sdPlayMP3(String filename)
{
    fs.begin();


    String filepath =
        String(fs.getRootPath()) + filename;


    File file = fs.open(filepath, MP3);


    // Volume
    file.setMp3DigitalVol(150);


    file.playMp3();


    file.close();

``` 
    fs.end();
}
