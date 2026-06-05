作業內容
6.Using MPU6050 to get heading angle
Code: MPU6050_DMP6_GetHeading.ino
Homework: exercise MPU6050
上傳截圖

成果
![成果1](../images/hw6.png)

程式碼:
```  
#include "I2Cdev.h"
#include <MPU6050_IMU_libraries/MPU6050_6Axis_MotionApps612.h>

#if I2CDEV_IMPLEMENTATION == I2CDEV_ARDUINO_WIRE
#include "Wire.h"
#endif

MPU6050 mpu;

// ===== DMP variables =====
bool dmpReady = false;
uint8_t devStatus;
uint16_t packetSize;
uint8_t fifoBuffer[64];

Quaternion q;
VectorFloat gravity;
float ypr[3];

int Heading = 0;

// ================= SETUP =================
void setup()
{
    Serial.begin(115200);
    while (!Serial);

    Serial.println("Initializing I2C...");

    Wire.begin();
    Wire.setClock(400000);

    Serial.println("Initializing MPU6050...");
    mpu.initialize();

    Serial.println("Testing connection...");
    Serial.println(mpu.testConnection() ? "MPU6050 OK" : "MPU6050 FAIL");

    Serial.println("Initializing DMP...");
    devStatus = mpu.dmpInitialize();

    // offsets（可用預設）
    mpu.setXGyroOffset(51);
    mpu.setYGyroOffset(8);
    mpu.setZGyroOffset(21);
    mpu.setXAccelOffset(1150);
    mpu.setYAccelOffset(-50);
    mpu.setZAccelOffset(1060);

    if (devStatus == 0)
    {
        mpu.setDMPEnabled(true);
        packetSize = mpu.dmpGetFIFOPacketSize();
        dmpReady = true;

        Serial.println("DMP READY!");
    }
    else
    {
        Serial.print("DMP FAIL: ");
        Serial.println(devStatus);
    }

    pinMode(LED_BUILTIN, OUTPUT);
}

// ================= LOOP =================
void loop()
{
    if (!dmpReady) return;

    if (mpu.dmpGetCurrentFIFOPacket(fifoBuffer))
    {
        mpu.dmpGetQuaternion(&q, fifoBuffer);
        mpu.dmpGetGravity(&gravity, &q);
        mpu.dmpGetYawPitchRoll(ypr, &q, &gravity);

        Heading = (int)(ypr[0] * 180 / M_PI) + 180;

        Serial.print("Yaw / Heading: ");
        Serial.println(Heading);

        digitalWrite(LED_BUILTIN, !digitalRead(LED_BUILTIN));
    }
}

```  
