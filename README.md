# V7RC-Protocol

## Languages
- [Traditional Chinese](#traditional-chinese)
- [English](#english)
- [简体中文](#简体中文)
- [日本語](#日本語)
- [ภาษาไทย](#ภาษาไทย)
- [Bahasa Melayu](#bahasa-melayu)
- [Bahasa Indonesia](#bahasa-indonesia)

## Traditional Chinese

### 開始之前
此儲存庫用來定義 V7RC Protocol，提供 V7RC APP 與相關 MCU 開發板開發時參考。通常 V7RC 團隊會先規劃並在 APP 端釋出新功能，之後 Makers 可以依照此文件開發自己的控制板或裝置。如果你在閱讀文件時有任何問題，歡迎與我們聯繫。

### 關於 V7RC Protocol
V7RC Protocol 的設計核心是以 BLE Payload 為基礎。由於需要透過 WIFI 與 BLE 將資料傳送到 MCU 開發板，因此協定必須盡量相容於常見的 WIFI 與 BLE 傳輸情境。每一筆 V7RC Payload 長度固定為 20 Bytes。V7RC APP 會依照此格式送出與接收封包，只要依照此規格實作，就能較容易地將裝置整合到 V7RC APP 中進行遠端控制。

### Payload 格式
範例：

`SRV1500150015001500#`

1. 長度必須為 20 Bytes（未來可能擴充）。
2. 前 3 Bytes 為命令字串，例如 `SRV` 代表控制 PWM 馬達通道。
3. 結尾固定為 `#`，方便辨識每一筆 Payload。
4. 中間 16 Bytes 為資料區，多數命令使用 ASCII 資料，少數命令會使用二進位概念資料，例如 `SS8`。

### 常見命令
- `SRV`
- `SRT`
- `SS8`

### 下一階段規劃
更多協定內容請參考 [protocol.md](/Users/louischuang/CompanyStorage/嵐奕科技有限公司/第四處 技術處/正在進行中的專案/2017_V7_RC_Lite/2026-v7rc-protocol/V7RC-Protocol/protocol.md)。

## English

### Before You Start
This repository defines the V7RC Protocol used by the V7RC APP and related MCU development boards. In most cases, the V7RC team plans and releases new APP features first, and makers can then use this document to build their own compatible boards and devices. If you have any questions while reading the document, please contact us.

### About the V7RC Protocol
The V7RC Protocol is designed around the BLE payload concept. Because data may be transmitted to MCU development boards through both WIFI and BLE, the protocol is designed to stay compatible with common WIFI and BLE transmission scenarios. Each V7RC payload is fixed at 20 bytes. The V7RC APP sends and receives packets in this format, so following this specification makes it much easier to integrate your device with the V7RC APP for remote control.

### Payload Format
Example:

`SRV1500150015001500#`

1. The payload length must be 20 bytes, although it may be extended in the future.
2. The first 3 bytes are the command string. In this example, `SRV` means PWM motor channel control.
3. The payload always ends with `#`, which makes each packet easy to identify.
4. The middle 16 bytes are the data field. Most commands use ASCII data, while some commands such as `SS8` use compact binary-like values.

### Common Commands
- `SRV`
- `SRT`
- `SS8`

### Next Phase
For the full protocol description, see [protocol.md](/Users/louischuang/CompanyStorage/嵐奕科技有限公司/第四處 技術處/正在進行中的專案/2017_V7_RC_Lite/2026-v7rc-protocol/V7RC-Protocol/protocol.md).

## 简体中文

### 开始之前
此仓库用于定义 V7RC Protocol，供 V7RC APP 与相关 MCU 开发板开发时参考。通常 V7RC 团队会先规划并在 APP 端发布新功能，之后 Makers 可以根据本文档开发自己的控制板或设备。如果你在阅读文档时有任何问题，欢迎与我们联系。

### 关于 V7RC Protocol
V7RC Protocol 的设计核心基于 BLE Payload。由于需要通过 WIFI 与 BLE 将数据传输到 MCU 开发板，因此协议必须尽量兼容常见的 WIFI 与 BLE 传输场景。每个 V7RC Payload 的长度固定为 20 Bytes。V7RC APP 会按照此格式发送与接收封包，只要依照此规范实现，就能更轻松地将设备整合到 V7RC APP 中进行远程控制。

### Payload 格式
示例：

`SRV1500150015001500#`

1. 长度必须为 20 Bytes，未来可能扩充。
2. 前 3 Bytes 为命令字符串，例如 `SRV` 表示控制 PWM 电机通道。
3. 结尾固定为 `#`，便于识别每一笔 Payload。
4. 中间 16 Bytes 为数据区，大多数命令使用 ASCII 数据，少数命令会使用类似二进制的紧凑数据，例如 `SS8`。

### 常见命令
- `SRV`
- `SRT`
- `SS8`

### 下一阶段规划
完整协议说明请参考 [protocol.md](/Users/louischuang/CompanyStorage/嵐奕科技有限公司/第四處 技術處/正在進行中的專案/2017_V7_RC_Lite/2026-v7rc-protocol/V7RC-Protocol/protocol.md)。

## 日本語

### はじめに
このリポジトリは、V7RC APP と関連 MCU 開発ボードで利用する V7RC Protocol を定義するためのものです。通常、V7RC チームが先に APP の新機能を企画して公開し、その後 Makers がこの文書を参照して独自の開発ボードやデバイスを実装します。文書の内容について質問があれば、お気軽にご連絡ください。

### V7RC Protocol について
V7RC Protocol は BLE のペイロード設計を基準にしています。MCU 開発ボードへ WIFI と BLE の両方でデータを送る必要があるため、一般的な WIFI / BLE 通信環境で扱いやすい構成になっています。各 V7RC Payload の長さは 20 Bytes 固定です。V7RC APP はこの形式でパケットを送受信するため、この仕様に従えば V7RC APP によるリモート制御へ比較的容易に対応できます。

### Payload 形式
例：

`SRV1500150015001500#`

1. 長さは 20 Bytes である必要があります。将来的に拡張される可能性があります。
2. 先頭 3 Bytes はコマンド文字列です。この例では `SRV` が PWM モーターチャンネル制御を意味します。
3. 末尾は必ず `#` で終わり、各 Payload を識別しやすくしています。
4. 中間の 16 Bytes はデータ領域です。多くのコマンドは ASCII データを使用し、`SS8` など一部のコマンドは圧縮表現の値を使用します。

### 主なコマンド
- `SRV`
- `SRT`
- `SS8`

### 次の段階
完全なプロトコル説明は [protocol.md](/Users/louischuang/CompanyStorage/嵐奕科技有限公司/第四處 技術處/正在進行中的專案/2017_V7_RC_Lite/2026-v7rc-protocol/V7RC-Protocol/protocol.md) を参照してください。

## ภาษาไทย

### ก่อนเริ่มต้น
ที่เก็บนี้ใช้สำหรับกำหนด V7RC Protocol ซึ่งใช้งานร่วมกับ V7RC APP และบอร์ดพัฒนา MCU ที่เกี่ยวข้อง โดยทั่วไปทีม V7RC จะวางแผนและปล่อยฟีเจอร์ใหม่บน APP ก่อน จากนั้น Makers สามารถอ้างอิงเอกสารนี้เพื่อพัฒนาบอร์ดหรืออุปกรณ์ของตนเองได้ หากคุณมีคำถามระหว่างอ่านเอกสาร โปรดติดต่อเราได้เสมอ

### เกี่ยวกับ V7RC Protocol
V7RC Protocol ออกแบบโดยอ้างอิงแนวคิดของ BLE Payload เนื่องจากต้องส่งข้อมูลไปยังบอร์ดพัฒนา MCU ผ่านทั้ง WIFI และ BLE โปรโตคอลจึงถูกออกแบบให้เข้ากันได้กับรูปแบบการรับส่งข้อมูลที่พบบ่อยของทั้งสองระบบ แต่ละ V7RC Payload มีความยาวคงที่ 20 Bytes และ V7RC APP จะรับส่งข้อมูลตามรูปแบบนี้ หากพัฒนาอุปกรณ์ตามสเปกนี้ จะสามารถเชื่อมต่อใช้งานกับ V7RC APP เพื่อควบคุมจากระยะไกลได้ง่ายขึ้น

### รูปแบบ Payload
ตัวอย่าง:

`SRV1500150015001500#`

1. ความยาวของ Payload ต้องเป็น 20 Bytes และอาจมีการขยายในอนาคต
2. 3 Bytes แรกคือคำสั่ง ตัวอย่างนี้ `SRV` หมายถึงการควบคุมช่อง PWM ของมอเตอร์
3. ปลายท้ายของ Payload ต้องเป็น `#` เพื่อให้แยกแต่ละแพ็กเก็ตได้ง่าย
4. ตรงกลาง 16 Bytes เป็นส่วนข้อมูล ส่วนใหญ่ใช้ข้อมูลแบบ ASCII และบางคำสั่ง เช่น `SS8` จะใช้ค่ารูปแบบย่อที่กะทัดรัดกว่า

### คำสั่งทั่วไป
- `SRV`
- `SRT`
- `SS8`

### แผนในระยะถัดไป
รายละเอียดโปรโตคอลฉบับเต็มดูได้ที่ [protocol.md](/Users/louischuang/CompanyStorage/嵐奕科技有限公司/第四處 技術處/正在進行中的專案/2017_V7_RC_Lite/2026-v7rc-protocol/V7RC-Protocol/protocol.md)

## Bahasa Melayu

### Sebelum Bermula
Repositori ini digunakan untuk mentakrifkan V7RC Protocol yang digunakan oleh V7RC APP dan papan pembangunan MCU yang berkaitan. Lazimnya pasukan V7RC akan merancang serta melepaskan ciri baharu pada APP terlebih dahulu, kemudian makers boleh merujuk dokumen ini untuk membangunkan papan atau peranti mereka sendiri. Jika anda mempunyai sebarang soalan semasa membaca dokumen ini, sila hubungi kami.

### Tentang V7RC Protocol
V7RC Protocol direka berasaskan konsep BLE Payload. Oleh sebab data perlu dihantar ke papan pembangunan MCU melalui WIFI dan BLE, protokol ini direka supaya serasi dengan kebanyakan senario penghantaran WIFI dan BLE. Setiap V7RC Payload mempunyai panjang tetap 20 Bytes. V7RC APP menghantar dan menerima paket dalam format ini, jadi dengan mengikuti spesifikasi ini, peranti anda boleh diintegrasikan dengan lebih mudah ke dalam V7RC APP untuk kawalan jauh.

### Format Payload
Contoh:

`SRV1500150015001500#`

1. Panjang payload mestilah 20 Bytes, dan mungkin diperluaskan pada masa hadapan.
2. 3 Bytes pertama ialah rentetan arahan. Dalam contoh ini, `SRV` bermaksud kawalan saluran motor PWM.
3. Payload sentiasa berakhir dengan `#`, supaya setiap paket mudah dikenal pasti.
4. 16 Bytes di bahagian tengah ialah ruang data. Kebanyakan arahan menggunakan data ASCII, manakala sesetengah arahan seperti `SS8` menggunakan nilai padat berbentuk ringkas.

### Arahan Biasa
- `SRV`
- `SRT`
- `SS8`

### Fasa Seterusnya
Untuk penerangan protokol penuh, rujuk [protocol.md](/Users/louischuang/CompanyStorage/嵐奕科技有限公司/第四處 技術處/正在進行中的專案/2017_V7_RC_Lite/2026-v7rc-protocol/V7RC-Protocol/protocol.md).

## Bahasa Indonesia

### Sebelum Memulai
Repositori ini digunakan untuk mendefinisikan V7RC Protocol yang dipakai oleh V7RC APP dan papan pengembangan MCU terkait. Biasanya tim V7RC akan merencanakan dan merilis fitur baru di APP terlebih dahulu, lalu makers dapat menggunakan dokumen ini untuk mengembangkan papan atau perangkat mereka sendiri. Jika Anda memiliki pertanyaan saat membaca dokumen ini, silakan hubungi kami.

### Tentang V7RC Protocol
V7RC Protocol dirancang berdasarkan konsep BLE Payload. Karena data perlu dikirim ke papan pengembangan MCU melalui WIFI dan BLE, protokol ini dibuat agar kompatibel dengan skenario transmisi WIFI dan BLE yang umum. Setiap V7RC Payload memiliki panjang tetap 20 Bytes. V7RC APP mengirim dan menerima paket dalam format ini, sehingga dengan mengikuti spesifikasi ini, perangkat Anda dapat lebih mudah diintegrasikan ke V7RC APP untuk kendali jarak jauh.

### Format Payload
Contoh:

`SRV1500150015001500#`

1. Panjang payload harus 20 Bytes dan mungkin diperluas di masa mendatang.
2. 3 Bytes pertama adalah string perintah. Pada contoh ini, `SRV` berarti kontrol kanal motor PWM.
3. Payload selalu diakhiri dengan `#` agar setiap paket mudah dikenali.
4. 16 Bytes di tengah adalah area data. Sebagian besar perintah menggunakan data ASCII, sedangkan beberapa perintah seperti `SS8` memakai nilai ringkas yang lebih padat.

### Perintah Umum
- `SRV`
- `SRT`
- `SS8`

### Tahap Berikutnya
Untuk deskripsi protokol lengkap, lihat [protocol.md](/Users/louischuang/CompanyStorage/嵐奕科技有限公司/第四處 技術處/正在進行中的專案/2017_V7_RC_Lite/2026-v7rc-protocol/V7RC-Protocol/protocol.md).
