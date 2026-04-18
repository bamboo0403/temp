# RFID Access Control System

## 概述

<details>
<summary>點擊展開查看系統概述</summary>

這是一個先進的基於 Arduino 的 RFID 門禁控制系統，整合了多種感測器和輸出裝置，以實現安全且智能的門禁管理。系統採用 RFID 卡片進行身份驗證，結合 PIR 運動感測器來增強安全性。使用者介面通過 16x2 LCD 顯示器提供即時狀態回饋，同時利用 RGB LED 和蜂鳴器提供多模態警示。

### 主要功能
- **RFID 身份驗證**：支援多張授權卡片的快速掃描和驗證
- **運動檢測**：PIR 感測器防止未授權進入
- **門控制**：伺服馬達驅動的自動門開關機制
- **視覺回饋**：RGB LED 狀態指示
- **聲音警示**：蜂鳴器提供音頻回饋
- **即時顯示**：LCD 螢幕顯示系統狀態和訊息

</details>

## 系統架構

<details>
<summary>點擊展開查看系統架構</summary>

系統採用模組化設計，各個元件通過 Arduino Uno 微控制器協調工作：

- **輸入模組**：RFID 讀取器、PIR 感測器
- **輸出模組**：LCD 顯示器、RGB LED、蜂鳴器、伺服馬達
- **通訊介面**：SPI (RFID)、I2C (LCD)、數位/類比 I/O

</details>

## 硬體需求

<details>
<summary>點擊展開查看硬體需求</summary>

### 核心元件
- **微控制器**：Arduino Uno R3 或相容板 (ATmega328P)
- **RFID 模組**：MFRC522 13.56MHz RFID/NFC 讀寫器
- **顯示器**：16x2 字符 LCD (I2C 介面，PCF8574 控制器，地址 0x3F)
- **致動器**：SG90 伺服馬達 (180° 旋轉)
- **感測器**：
  - PIR 運動感測器 (HC-SR501)
- **指示器**：
  - RGB LED (共陰極，支援 PWM 調光)
  - 單色 LED (電源指示)
- **音頻裝置**：無源蜂鳴器 (支援 PWM 音調控制)

### 輔助材料
- 杜邦線 (公對母、母對母)
- 麵包板
- 外部電源供應器 (推薦 9V DC 適配器)
- RFID 卡片/標籤 (MIFARE Classic 1K)

</details>



### Arduino Uno 接腳配置

<details>
<summary>點擊展開查看詳細接腳連接</summary>

#### RFID MFRC522
| 接腳 | Arduino Pin | 說明 |
|------|-------------|------|
| SDA (SS) | 10 | SPI Slave Select (注意：與 Green LED 共用) |
| SCK | 13 | SPI Serial Clock |
| MOSI | 11 | SPI Master Out Slave In |
| MISO | 12 | SPI Master In Slave Out |
| IRQ | - | 未使用 |
| RST | A0 | Reset 信號 |
| VCC | 3.3V | 電源供應 (3.3V) |
| GND | GND | 接地 |

#### LCD 顯示器 (I2C)
| 接腳 | Arduino Pin | 說明 |
|------|-------------|------|
| SDA | A4 | I2C 資料線 |
| SCL | A5 | I2C 時鐘線 |
| VCC | 5V | 電源供應 (5V) |
| GND | GND | 接地 |

#### 伺服馬達
| 接腳 | Arduino Pin | 說明 |
|------|-------------|------|
| Signal | 9 | PWM 控制信號 |
| VCC | 5V | 電源供應 |
| GND | GND | 接地 |

#### PIR 運動感測器
| 接腳 | Arduino Pin | 說明 |
|------|-------------|------|
| Signal | 2 | 數位輸出 (HIGH = 運動檢測) |
| VCC | 5V | 電源供應 |
| GND | GND | 接地 |

#### RGB LED (共陰極)
| 接腳 | Arduino Pin | 說明 |
|------|-------------|------|
| Red | 6 | 紅色通道 (PWM) |
| Green | 5 | 綠色通道 (PWM) |
| Blue | 3 | 藍色通道 (PWM) |
| GND | GND | 共陰極接地 |

#### Power LED
| 接腳 | Arduino Pin | 說明 |
|------|-------------|------|
| Anode (+) | 13 | 正極 (數位輸出) |
| Cathode (-) | GND | 負極接地 |

#### 蜂鳴器
| 接腳 | Arduino Pin | 說明 |
|------|-------------|------|
| Signal | 8 | 控制信號 (PWM 產生音調) |
| GND | GND | 接地 |

</details>



## 軟體環境

<details>
<summary>點擊展開查看軟體環境</summary>

### 開發環境
- **PlatformIO** (推薦) 或 Arduino IDE
- **作業系統**：Windows 10/11, macOS, Linux

### 依賴函式庫
安裝以下 Arduino 函式庫 (通過 Library Manager 或 platformio.ini)：

```ini
lib_deps =
    LiquidCrystal_PCF8574
    MFRC522
    Servo
```

</details>

### 建置與上傳

<details>
<summary>點擊展開查看建置步驟</summary>

> [!TIP]
> **開發建議**：使用 PlatformIO 可以獲得更好的依賴管理、自動程式庫安裝和跨平台支援。

1. **環境設置**：
   ```bash
   # 安裝 PlatformIO (如果尚未安裝)
   pip install platformio

   # 進入專案目錄
   cd /path/to/project
   ```

2. **編譯專案**：
   ```bash
   platformio run
   ```

3. **上傳至 Arduino**：
   ```bash
   platformio run --target upload
   ```

4. **監控序列輸出** (可選)：
   ```bash
   platformio run --target monitor
   ```

</details>

## 組態與自訂

<details>
<summary>點擊展開查看組態選項</summary>

> [!IMPORTANT]
> **組態提醒**：修改程式碼前請先備份，並在測試環境中驗證變更。

### RFID 卡片授權
在 `src/rfid/rfid.cpp` 中修改授權 UID 列表：

```cpp
if (content == "32:F8:E1:1A" ||
    content == "12:94:AE:1B" ||
    content == "YOUR_NEW_UID") {
    // 授權邏輯
}
```

### 系統參數調整
- **門開啟角度**：修改 `servo.h` 中的角度值
- **延遲時間**：調整 `rfid.cpp` 中的 `delay()` 值
- **LCD 背光**：修改 `main.cpp` 中的背光設定
- **LED 顏色**：自訂 `led.cpp` 中的顏色函數

</details>

## 操作指南

<details>
<summary>點擊展開查看操作指南</summary>

### 系統啟動
1. 連接所有硬體元件
2. 上傳程式碼至 Arduino
3. 電源啟動系統
4. LCD 顯示 "Waiting for card"

### 正常操作流程
1. **卡片掃描**：將授權 RFID 卡片靠近讀取器
2. **身份驗證**：系統檢查 UID 是否在授權列表中
3. **安全檢查**：驗證 PIR 感測器未檢測到異常運動
4. **門控制**：
   - 成功：門開啟 5 秒，LCD 顯示歡迎訊息
   - 失敗：LCD 顯示錯誤訊息，蜂鳴器警示

### 狀態指示
- **LCD 訊息**：
  - "Waiting for card" - 等待卡片
  - "UID: XX:XX:XX:XX" - 顯示卡片 UID
  - "*** Welcome ***" - 授權成功
  - "Danger!!Danger!!" - 安全威脅
  - "*** Error!!! ***" - 授權失敗

- **LED 顏色**：
  - 紫色：授權成功
  - 紅色：授權失敗
  - 青色：安全警示
  - 白色：閃爍警示

- **音頻回饋**：
  - 短蜂鳴：門開啟
  - 長蜂鳴：錯誤警示

</details>

## 故障排除

<details>
<summary>點擊展開查看故障排除指南</summary>

### 常見問題

#### LCD 不顯示
- 檢查 I2C 地址 (預設 0x3F)
- 驗證 SDA/SCL 連接 (A4/A5)
- 確認電源供應 (5V)

#### RFID 不讀取
- 檢查 SPI 連接 (Pins 10-13)
- 驗證 RFID 模組電源 (3.3V)
- 測試 RST 接腳 (A0)

#### 伺服馬達不動
- 檢查電源供應
- 驗證控制接腳 (Pin 9)
- 確認馬達型號相容性

#### PIR 感測器異常
- 調整感測器靈敏度電位器
- 檢查接腳連接 (Pin 2)
- 避免感測器附近強光源

#### 程式編譯錯誤
- 安裝所有必要函式庫
- 檢查 PlatformIO 環境配置
- 驗證 Arduino 板型設定

### 除錯技巧
- 使用序列監控器觀察除錯輸出
- 逐步測試各個元件功能
- 檢查接腳定義與實際連接一致性

</details>

## 效能規格

<details>
<summary>點擊展開查看效能規格</summary>

- **響應時間**：< 500ms (RFID 讀取)
- **電源消耗**：< 200mA (峰值)
- **工作溫度**：0°C - 50°C
- **支援卡片**：MIFARE Classic 1K/4K, NTAG

</details>

## 安全注意事項

<details>
<summary>點擊展開查看安全注意事項</summary>

> [!CAUTION]
> **安全警示**：此系統僅供教育和原型開發使用，不適用於商業安全應用。請勿在關鍵安全場景中使用。

- 確保所有連接牢固，避免短路
- 使用適當電源供應，避免過載
- RFID 系統僅提供基本安全層級
- 定期檢查感測器校準
- 避免在潮濕環境使用

</details>

## 擴展與修改

<details>
<summary>點擊展開查看擴展建議</summary>

### 潛在改進
- 添加網路連線 (WiFi/Ethernet)
- 整合資料庫儲存授權清單
- 實作時間-based 存取控制
- 添加語音提示功能
- 整合手機 App 控制

### 自訂開發
- 修改 `rfid.cpp` 添加新功能
- 調整 `led.h` 中的顏色定義
- 擴展 `sensors.h` 新增感測器
- 自訂 LCD 訊息在 `lcd.h`

</details>

