# RFID Access Control System

這是一個基於 Arduino 的 RFID 門禁系統，使用 RFID 卡片進行身份驗證，結合 PIR 感測器和超音波感測器來控制門的開關。系統會在 LCD 上顯示狀態，並使用 LED 和蜂鳴器提供視覺和聲音回饋。

## 硬體需求

- Arduino Uno 或相容板
- RFID 模組 (MFRC522)
- 16x2 LCD 顯示器 (I2C 介面，地址 0x3F)
- SG90 伺服馬達
- PIR 運動感測器
- HC-SR04 超音波感測器
- RGB LED (共陰極)
- 單色 LED (Power LED)
- 無源蜂鳴器
- 跳線和麵包板

## 接腳連接

### Arduino Uno 接腳配置

| 元件          | 接腳 | 說明                  |
|---------------|------|-----------------------|
| RFID MFRC522 SDA (SS) | 10   | SPI Slave Select      |
| RFID MFRC522 SCK     | 13   | SPI Clock             |
| RFID MFRC522 MOSI    | 11   | SPI Master Out Slave In |
| RFID MFRC522 MISO    | 12   | SPI Master In Slave Out |
| RFID MFRC522 RST     | A0   | Reset                 |
| RFID MFRC522 VCC     | 3.3V | 電源                  |
| RFID MFRC522 GND     | GND  | 接地                  |
| LCD SDA              | A4   | I2C Data              |
| LCD SCL              | A5   | I2C Clock             |
| LCD VCC              | 5V   | 電源                  |
| LCD GND              | GND  | 接地                  |
| Servo 馬達           | 9    | PWM 控制              |
| PIR 感測器           | 2    | 數位輸入              |
| 超音波 Trig          | 6    | 觸發                  |
| 超音波 Echo          | 5    | 回波                  |
| RGB LED Red          | 11   | PWM                   |
| RGB LED Green        | 10   | 注意：與 RFID SDA 共用，可能需要調整 |
| RGB LED Blue         | 3    | PWM                   |
| RGB LED GND          | GND  | 共陰極接地            |
| Power LED            | 13   | 數位輸出              |
| 蜂鳴器               | 8    | 數位輸出              |

**注意：** Green LED 和 RFID SDA 都使用 Pin 10，這可能導致衝突。請檢查您的電路並調整 pin 定義。

## 軟體設置

1. 安裝 PlatformIO 或 Arduino IDE。
2. 安裝必要的函式庫：
   - LiquidCrystal_PCF8574
   - MFRC522
   - Servo
   - DHT (如果使用溫濕度感測器)
3. 複製程式碼到您的專案。
4. 上傳程式到 Arduino。

## 使用說明

1. 電源啟動後，LCD 會顯示 "Waiting for card" 和 "Please scan RFID"。
2. 將授權的 RFID 卡片靠近讀取器。
3. 如果卡片授權且 PIR 感測器未檢測到運動，門會開啟，LCD 顯示歡迎訊息。
4. 如果 PIR 檢測到運動，系統會顯示危險訊息並拒絕開門。
5. 5 秒後門自動關閉。
6. 未授權卡片會顯示錯誤訊息。

## 授權卡片

目前授權的 UID：
- 32:F8:E1:1A
- 12:94:AE:1B

要添加更多卡片，請修改 `rfid.cpp` 中的 `if` 條件。

## 故障排除

- 如果 LCD 不顯示，檢查 I2C 地址和連接。
- 如果 RFID 不讀取，檢查 SPI 連接和電源。
- 如果 Servo 不動，檢查電源和 PWM 接腳。
