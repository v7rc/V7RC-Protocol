# V7RC-Protocol

[Language Index](README.md)

## 開始之前
此儲存庫用來定義 V7RC Protocol，提供 V7RC APP 與相關 MCU 開發板開發時參考。通常 V7RC 團隊會先規劃並在 APP 端釋出新功能，之後 Makers 可以依照此文件開發自己的控制板或裝置。如果你在閱讀文件時有任何問題，歡迎與我們聯繫。

## 關於 V7RC Protocol
V7RC Protocol 的設計核心是以 BLE Payload 為基礎。由於需要透過 WIFI 與 BLE 將資料傳送到 MCU 開發板，因此協定必須盡量相容於常見的 WIFI 與 BLE 傳輸情境。每一筆 V7RC Payload 長度固定為 20 Bytes。V7RC APP 會依照此格式送出與接收封包，只要依照此規格實作，就能較容易地將裝置整合到 V7RC APP 中進行遠端控制。

## Payload 格式
範例：

`SRV1500150015001500#`

1. 長度必須為 20 Bytes（未來可能擴充）。
2. 前 3 Bytes 為命令字串，例如 `SRV` 代表控制 PWM 馬達通道。
3. 結尾固定為 `#`，方便辨識每一筆 Payload。
4. 中間 16 Bytes 為資料區，多數命令使用 ASCII 資料，少數命令會使用二進位概念資料，例如 `SS8`。

## 常見命令
- `SRV`
- `SRT`
- `SS8`

## 下一階段規劃
更多協定內容請參考 [protocol.zh-TW.md](protocol.zh-TW.md)。

