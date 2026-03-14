# V7RC IO Command Protocol 說明

## Languages
- [繁體中文](#繁體中文)
- [English](#english)
- [简体中文](#简体中文)
- [日本語](#日本語)
- [ภาษาไทย](#ภาษาไทย)
- [Bahasa Melayu](#bahasa-melayu)
- [Bahasa Indonesia](#bahasa-indonesia)

## 繁體中文

因應更多的應用需求，V7RC 開放的 Protocol 已進行修正，以達到更好的相容性。實際支援度可能會因不同的開發板與韌體版本而有所不同。

### 基本規範

因應藍牙 BLE 的傳輸封包長度限制，命令長度統一限制在 **20 Bytes 以內**。
目前主要支援 20 Bytes 規格，16 Bytes 版本在新版 APP 中已不再支援。

- **命令格式**：前三碼統一表示命令代碼。
- **結尾符號**：統一以 `#` 表示。
- **空白補齊**：沒有使用到的欄位統一以 `0` 補滿，`CMD` 指令則以空白字元補足。

### 控制指令詳細說明

#### A. `SRV`（基本 PWM 控制）
- **說明**：標準 PWM 控制命令，長度為 20 Bytes。
- **範例**：
- `SRV1500150015001500#`：支援 P0、P1、P2 的 PWM 訊號，Microbit 標準版目前通常支援三個 PWM。
- `SRV1500100018002000#`：支援 C1、C2、C3、C4 的 PWM 訊號。
- **注意**：16 Bytes 版本如 `SRV150015001500#` 在新版 APP 中已不再支援，請避免使用。

#### B. `SR2`（第二組 PWM 控制）
- **說明**：如果開發板提供超過 4 個 PWM 通道，此命令可控制 C5 至 C8 的伺服馬達。
- **範例**：
- `SR21500100018002000#`：對應 C5、C6、C7、C8 的 PWM 訊號。

#### C. `SS8`（簡易 PWM，可同時使用 8 個 PWM）
- **說明**：將每個 PWM 值簡化為 `00` 到 `FF` 的十六進位數值，精細度較低。計算方式為先轉成十進位，再直接乘以 10 作為 PWM 數值。
- **範例**：
- `SS896969696969696#`：`96` 轉成十進位為 150，再乘以 10 後得到 1500。

#### D. `SSD`（簡易控制裝置加上按鈕控制，未實作）
- **說明**：每組 PWM 以三碼表示角度，最後四碼表示四個 GPIO 狀態，使用 `0` 或 `1` 表示。
- **範例**：
- `SSD0901801800001100#`
- C1 為 90 度、C2 為 180 度、C3 為 180 度、C4 為 0 度。
- GPIO0 為 ON、GPIO1 為 ON、GPIO2 為 OFF、GPIO3 為 OFF。

#### E. `SRT`（基本 PWM 的變形，Tank 控制）
- **說明**：由基本 PWM 指令變形而來，CH1 與 CH2 會在韌體端轉換成 Tank 控制訊號。
- **範例**：
- `SRT1500150015001500#`

#### F. `SST`（簡易控制裝置的變形，Tank 控制，未實作）
- **說明**：由簡易控制裝置變形而來，CH1 與 CH2 會在韌體端轉換成 Tank 控制訊號。
- **範例**：
- `SST0901801800001100#`

### 燈光控制指令（搭配 WS2812 晶片 LED 使用）

每顆 LED 以 `RGBM` 四個變數表示：

- **R（紅色）**：以 `0` 到 `F` 表示。
- **G（綠色）**：以 `0` 到 `F` 表示。
- **B（藍色）**：以 `0` 到 `F` 表示。
- **M（閃爍動作）**：以每秒亮燈時間表示。`0` 代表不亮，`1` 代表每秒亮 100ms，以此類推；`A` 以上代表恆亮。
- 例如 `F00A` 代表紅燈恆亮，`FFF5` 代表白燈每秒亮 500ms、暗 500ms。

#### G. `LED`（第一組四顆 LED）
- **格式**：20 Bytes。
- **範例**：
- `LED0111100000111110000022222#`：控制前方的 LED。

#### H. `LE2`（第二組四顆 LED）
- **格式**：20 Bytes。
- **範例**：
- `LE20111100000111110000022222#`：控制第二組 LED。

#### I. `LE?`（第 N 組四顆 LED）
- **說明**：`?` 代表數字 `N`，用來控制第 N 組四顆 LED。
- **範例**：
- `LE50111100000111110000022222#`：代表第五組 LED。

### 裝置自訂指令

#### J. `CMD`（傳遞給 Device 的控制命令）
- **說明**：可將命令傳遞給裝置端，由裝置端開發者自行定義其意義。最大長度為 16 個字元，不包含前綴與結尾符號。若不足 16 字元，需使用空白字元補足。
- **範例**：
- 若指令為 `RESET`，完整封包為 `CMDRESET          #`。

## English

The open V7RC Protocol has been revised to support more application scenarios and improve compatibility. Actual support may still vary depending on the development board and firmware version.

### Basic Rules

Because BLE transmission packets have a limited size, each command is restricted to **20 bytes or less**.
The current main format is 20 bytes, and the older 16-byte format is no longer supported in the newer APP.

- **Command format**: The first 3 characters are the command code.
- **Ending marker**: Every packet ends with `#`.
- **Padding rule**: Unused fields are padded with `0`, while the `CMD` instruction uses spaces for padding.

### Detailed Control Commands

#### A. `SRV` (Basic PWM Control)
- **Description**: Standard PWM control command with a total length of 20 bytes.
- **Examples**:
- `SRV1500150015001500#`: Supports PWM signals for P0, P1, and P2. The standard Microbit version typically supports three PWM outputs.
- `SRV1500100018002000#`: Supports PWM signals for C1, C2, C3, and C4.
- **Note**: The old 16-byte version such as `SRV150015001500#` is no longer supported by the newer APP and should not be used.

#### B. `SR2` (Second PWM Group)
- **Description**: If the board provides more than four PWM channels, this command controls servo channels C5 to C8.
- **Example**:
- `SR21500100018002000#`: Corresponds to PWM signals for C5, C6, C7, and C8.

#### C. `SS8` (Simplified PWM for 8 Channels)
- **Description**: Each PWM value is simplified into a hexadecimal value from `00` to `FF`, with lower precision. Convert the value to decimal first, then multiply it by 10 to get the PWM value.
- **Example**:
- `SS896969696969696#`: `96` becomes decimal 150, and 150 multiplied by 10 becomes 1500.

#### D. `SSD` (Simplified Device Control with Buttons, Not Implemented)
- **Description**: Each PWM output uses 3 digits for angle control, and the last 4 digits represent four GPIO states using `0` or `1`.
- **Example**:
- `SSD0901801800001100#`
- C1 is 90 degrees, C2 is 180 degrees, C3 is 180 degrees, and C4 is 0 degrees.
- GPIO0 is ON, GPIO1 is ON, GPIO2 is OFF, and GPIO3 is OFF.

#### E. `SRT` (Tank Mode Based on Basic PWM)
- **Description**: A variation of the basic PWM command. CH1 and CH2 are converted in firmware into tank-control signals.
- **Example**:
- `SRT1500150015001500#`

#### F. `SST` (Tank Mode Based on Simplified Device Control, Not Implemented)
- **Description**: A variation of the simplified device control command. CH1 and CH2 are converted in firmware into tank-control signals.
- **Example**:
- `SST0901801800001100#`

### Lighting Commands (for WS2812 LED Devices)

Each LED uses four variables in the form `RGBM`:

- **R (Red)**: A value from `0` to `F`.
- **G (Green)**: A value from `0` to `F`.
- **B (Blue)**: A value from `0` to `F`.
- **M (Blink mode)**: The lighting time per second. `0` means always off, `1` means on for 100ms each second, and so on. Values `A` and above mean always on.
- For example, `F00A` means solid red, and `FFF5` means white light on for 500ms and off for 500ms every second.

#### G. `LED` (First Group of Four LEDs)
- **Format**: 20 bytes.
- **Example**:
- `LED0111100000111110000022222#`: Controls the front LED group.

#### H. `LE2` (Second Group of Four LEDs)
- **Format**: 20 bytes.
- **Example**:
- `LE20111100000111110000022222#`: Controls the second LED group.

#### I. `LE?` (Nth Group of Four LEDs)
- **Description**: `?` is replaced by the group number `N` to control the Nth group of four LEDs.
- **Example**:
- `LE50111100000111110000022222#`: Represents the fifth LED group.

### Custom Device Commands

#### J. `CMD` (Pass-Through Command for the Device)
- **Description**: This command passes data directly to the device side, and its meaning is fully defined by the device developer. The maximum command length is 16 characters, not including the prefix and ending marker. If the content is shorter than 16 characters, pad it with spaces.
- **Example**:
- If the command is `RESET`, the full packet is `CMDRESET          #`.

## 简体中文

为满足更多应用需求，V7RC 开放的 Protocol 已进行调整，以获得更好的兼容性。实际支持情况仍可能因不同开发板与固件版本而有所不同。

### 基本规范

由于蓝牙 BLE 的传输封包长度有限，命令长度统一限制在 **20 Bytes 以内**。
目前主要支持 20 Bytes 规格，旧版 16 Bytes 格式在新版 APP 中已不再支持。

- **命令格式**：前 3 个字符表示命令代码。
- **结尾符号**：所有封包统一以 `#` 结尾。
- **补齐规则**：未使用的字段统一以 `0` 补满，`CMD` 指令则使用空格补齐。

### 控制指令详细说明

#### A. `SRV`（基本 PWM 控制）
- **说明**：标准 PWM 控制命令，总长度为 20 Bytes。
- **示例**：
- `SRV1500150015001500#`：支持 P0、P1、P2 的 PWM 信号，Microbit 标准版通常支持三个 PWM 输出。
- `SRV1500100018002000#`：支持 C1、C2、C3、C4 的 PWM 信号。
- **注意**：旧版 16 Bytes 格式如 `SRV150015001500#` 在新版 APP 中已不再支持，请勿继续使用。

#### B. `SR2`（第二组 PWM 控制）
- **说明**：若开发板提供超过 4 个 PWM 通道，此命令可控制 C5 到 C8 的伺服电机。
- **示例**：
- `SR21500100018002000#`：对应 C5、C6、C7、C8 的 PWM 信号。

#### C. `SS8`（简易 PWM，可同时使用 8 个 PWM）
- **说明**：将每个 PWM 值简化为 `00` 到 `FF` 的十六进制数值，精细度较低。先将其转换为十进制，再乘以 10 得到 PWM 值。
- **示例**：
- `SS896969696969696#`：`96` 转成十进制后是 150，再乘以 10 得到 1500。

#### D. `SSD`（简易装置控制加按钮控制，未实现）
- **说明**：每组 PWM 以 3 位数字表示角度，最后 4 位表示 4 个 GPIO 状态，使用 `0` 或 `1` 表示。
- **示例**：
- `SSD0901801800001100#`
- C1 为 90 度，C2 为 180 度，C3 为 180 度，C4 为 0 度。
- GPIO0 为 ON，GPIO1 为 ON，GPIO2 为 OFF，GPIO3 为 OFF。

#### E. `SRT`（基于基本 PWM 的 Tank 控制）
- **说明**：这是基本 PWM 指令的变形，CH1 与 CH2 会在固件端转换为 Tank 控制信号。
- **示例**：
- `SRT1500150015001500#`

#### F. `SST`（基于简易装置控制的 Tank 控制，未实现）
- **说明**：这是简易装置控制指令的变形，CH1 与 CH2 会在固件端转换为 Tank 控制信号。
- **示例**：
- `SST0901801800001100#`

### 灯光控制指令（适用于 WS2812 LED）

每颗 LED 由 `RGBM` 四个变量表示：

- **R（红色）**：取值范围 `0` 到 `F`。
- **G（绿色）**：取值范围 `0` 到 `F`。
- **B（蓝色）**：取值范围 `0` 到 `F`。
- **M（闪烁模式）**：表示每秒的亮灯时间。`0` 表示常灭，`1` 表示每秒亮 100ms，以此类推；`A` 以上表示常亮。
- 例如 `F00A` 表示红灯常亮，`FFF5` 表示白灯每秒亮 500ms、灭 500ms。

#### G. `LED`（第一组 4 颗 LED）
- **格式**：20 Bytes。
- **示例**：
- `LED0111100000111110000022222#`：控制前方 LED 组。

#### H. `LE2`（第二组 4 颗 LED）
- **格式**：20 Bytes。
- **示例**：
- `LE20111100000111110000022222#`：控制第二组 LED。

#### I. `LE?`（第 N 组 4 颗 LED）
- **说明**：`?` 代表数字 `N`，用于控制第 N 组 4 颗 LED。
- **示例**：
- `LE50111100000111110000022222#`：表示第五组 LED。

### 设备自定义指令

#### J. `CMD`（传递给设备端的控制命令）
- **说明**：该命令会将数据直接传递给设备端，其含义由设备开发者自行定义。命令最大长度为 16 个字符，不包含前缀和结尾符号。若不足 16 个字符，需要使用空格补齐。
- **示例**：
- 若命令为 `RESET`，完整封包为 `CMDRESET          #`。

## 日本語

より多くの用途に対応するため、公開されている V7RC Protocol は互換性向上を目的として調整されています。実際の対応状況は、開発ボードやファームウェアのバージョンによって異なる場合があります。

### 基本仕様

BLE の通信パケット長の制限に合わせて、各コマンドの長さは **20 Bytes 以内** に統一されています。
現在の主な仕様は 20 Bytes 版であり、旧 16 Bytes 版は新しい APP ではサポートされません。

- **コマンド形式**：先頭 3 文字がコマンドコードです。
- **終端記号**：すべてのパケットは `#` で終わります。
- **補完ルール**：未使用フィールドは `0` で埋め、`CMD` 命令のみ空白文字で埋めます。

### 制御コマンド詳細

#### A. `SRV`（基本 PWM 制御）
- **説明**：標準的な PWM 制御コマンドで、全長は 20 Bytes です。
- **例**：
- `SRV1500150015001500#`：P0、P1、P2 の PWM 信号に対応します。Microbit 標準版は通常 3 系統の PWM に対応します。
- `SRV1500100018002000#`：C1、C2、C3、C4 の PWM 信号に対応します。
- **注意**：`SRV150015001500#` のような旧 16 Bytes 版は新しい APP でサポートされないため、使用しないでください。

#### B. `SR2`（第 2 PWM グループ）
- **説明**：開発ボードが 4 つを超える PWM チャンネルを備える場合、このコマンドで C5 から C8 のサーボを制御します。
- **例**：
- `SR21500100018002000#`：C5、C6、C7、C8 の PWM 信号に対応します。

#### C. `SS8`（簡易 PWM、8 チャンネル同時制御）
- **説明**：各 PWM 値を `00` から `FF` の 16 進数に簡略化します。精度は下がりますが、値を 10 進数に変換して 10 倍することで PWM 値を得られます。
- **例**：
- `SS896969696969696#`：`96` は 10 進数で 150、さらに 10 倍して 1500 になります。

#### D. `SSD`（ボタン付き簡易デバイス制御、未実装）
- **説明**：各 PWM は 3 桁で角度を表し、最後の 4 桁は 4 つの GPIO 状態を `0` または `1` で表します。
- **例**：
- `SSD0901801800001100#`
- C1 は 90 度、C2 は 180 度、C3 は 180 度、C4 は 0 度です。
- GPIO0 は ON、GPIO1 は ON、GPIO2 は OFF、GPIO3 は OFF です。

#### E. `SRT`（基本 PWM 派生の Tank 制御）
- **説明**：基本 PWM 命令の派生版です。CH1 と CH2 はファームウェア側で Tank 制御信号に変換されます。
- **例**：
- `SRT1500150015001500#`

#### F. `SST`（簡易デバイス制御派生の Tank 制御、未実装）
- **説明**：簡易デバイス制御命令の派生版です。CH1 と CH2 はファームウェア側で Tank 制御信号に変換されます。
- **例**：
- `SST0901801800001100#`

### ライト制御コマンド（WS2812 LED 用）

各 LED は `RGBM` の 4 変数で表現されます。

- **R（赤）**：`0` から `F`。
- **G（緑）**：`0` から `F`。
- **B（青）**：`0` から `F`。
- **M（点滅モード）**：1 秒あたりの点灯時間です。`0` は消灯、`1` は毎秒 100ms 点灯、以後同様です。`A` 以上は常時点灯を意味します。
- 例として、`F00A` は赤の常時点灯、`FFF5` は白色が毎秒 500ms 点灯し 500ms 消灯する状態を意味します。

#### G. `LED`（最初の 4 LED グループ）
- **形式**：20 Bytes。
- **例**：
- `LED0111100000111110000022222#`：前方の LED グループを制御します。

#### H. `LE2`（第 2 の 4 LED グループ）
- **形式**：20 Bytes。
- **例**：
- `LE20111100000111110000022222#`：第 2 LED グループを制御します。

#### I. `LE?`（第 N の 4 LED グループ）
- **説明**：`?` には数字 `N` が入り、第 N グループの 4 つの LED を制御します。
- **例**：
- `LE50111100000111110000022222#`：第 5 LED グループを表します。

### デバイス独自コマンド

#### J. `CMD`（デバイスへ直接渡すコマンド）
- **説明**：このコマンドはデバイス側へデータをそのまま渡し、意味はデバイス開発者が自由に定義します。最大長は 16 文字で、接頭辞と終端記号は含みません。16 文字未満の場合は空白で埋めます。
- **例**：
- コマンドが `RESET` の場合、完全なパケットは `CMDRESET          #` です。

## ภาษาไทย

เพื่อรองรับการใช้งานที่หลากหลายมากขึ้น V7RC Protocol แบบเปิดได้ถูกปรับปรุงเพื่อเพิ่มความเข้ากันได้ อย่างไรก็ตามระดับการรองรับจริงอาจแตกต่างกันไปตามบอร์ดพัฒนาและเวอร์ชันของเฟิร์มแวร์

### ข้อกำหนดพื้นฐาน

เนื่องจากแพ็กเก็ตของ BLE มีข้อจำกัดด้านขนาด คำสั่งแต่ละชุดจึงถูกจำกัดไว้ที่ **ไม่เกิน 20 Bytes**
รูปแบบหลักที่รองรับในปัจจุบันคือ 20 Bytes ส่วนรูปแบบเก่า 16 Bytes ไม่รองรับใน APP รุ่นใหม่แล้ว

- **รูปแบบคำสั่ง**: 3 ตัวอักษรแรกคือรหัสคำสั่ง
- **สัญลักษณ์ปิดท้าย**: ทุกแพ็กเก็ตต้องลงท้ายด้วย `#`
- **กฎการเติมข้อมูล**: ช่องที่ไม่ได้ใช้งานให้เติม `0` ส่วนคำสั่ง `CMD` ให้เติมด้วยช่องว่าง

### รายละเอียดคำสั่งควบคุม

#### A. `SRV` (ควบคุม PWM พื้นฐาน)
- **คำอธิบาย**: คำสั่ง PWM มาตรฐาน มีความยาวรวม 20 Bytes
- **ตัวอย่าง**:
- `SRV1500150015001500#`: รองรับสัญญาณ PWM สำหรับ P0, P1 และ P2 โดย Microbit รุ่นมาตรฐานมักรองรับ PWM 3 ช่อง
- `SRV1500100018002000#`: รองรับสัญญาณ PWM สำหรับ C1, C2, C3 และ C4
- **หมายเหตุ**: รูปแบบเก่า 16 Bytes เช่น `SRV150015001500#` ไม่รองรับใน APP รุ่นใหม่แล้วและไม่ควรใช้งาน

#### B. `SR2` (ชุด PWM กลุ่มที่สอง)
- **คำอธิบาย**: หากบอร์ดมีช่อง PWM มากกว่า 4 ช่อง คำสั่งนี้ใช้ควบคุมเซอร์โว C5 ถึง C8
- **ตัวอย่าง**:
- `SR21500100018002000#`: ตรงกับสัญญาณ PWM ของ C5, C6, C7 และ C8

#### C. `SS8` (PWM แบบย่อ ใช้ได้พร้อมกัน 8 ช่อง)
- **คำอธิบาย**: ลดค่าของ PWM แต่ละช่องให้อยู่ในรูปเลขฐานสิบหก `00` ถึง `FF` ซึ่งมีความละเอียดต่ำกว่า โดยแปลงค่าเป็นเลขฐานสิบก่อนแล้วคูณ 10 เพื่อให้ได้ค่า PWM
- **ตัวอย่าง**:
- `SS896969696969696#`: `96` เมื่อแปลงเป็นเลขฐานสิบจะได้ 150 แล้วคูณ 10 เป็น 1500

#### D. `SSD` (ควบคุมอุปกรณ์แบบย่อพร้อมปุ่มกด, ยังไม่รองรับ)
- **คำอธิบาย**: PWM แต่ละชุดใช้ตัวเลข 3 หลักแทนมุม และ 4 หลักสุดท้ายแทนสถานะ GPIO 4 ช่องด้วย `0` หรือ `1`
- **ตัวอย่าง**:
- `SSD0901801800001100#`
- C1 เท่ากับ 90 องศา, C2 เท่ากับ 180 องศา, C3 เท่ากับ 180 องศา และ C4 เท่ากับ 0 องศา
- GPIO0 เป็น ON, GPIO1 เป็น ON, GPIO2 เป็น OFF และ GPIO3 เป็น OFF

#### E. `SRT` (Tank Control ที่ดัดแปลงจาก PWM พื้นฐาน)
- **คำอธิบาย**: เป็นรูปแบบดัดแปลงจากคำสั่ง PWM พื้นฐาน โดย CH1 และ CH2 จะถูกแปลงเป็นสัญญาณควบคุมแบบ Tank ที่ฝั่งเฟิร์มแวร์
- **ตัวอย่าง**:
- `SRT1500150015001500#`

#### F. `SST` (Tank Control ที่ดัดแปลงจากคำสั่งแบบย่อ, ยังไม่รองรับ)
- **คำอธิบาย**: เป็นรูปแบบดัดแปลงจากคำสั่งควบคุมอุปกรณ์แบบย่อ โดย CH1 และ CH2 จะถูกแปลงเป็นสัญญาณควบคุมแบบ Tank ที่ฝั่งเฟิร์มแวร์
- **ตัวอย่าง**:
- `SST0901801800001100#`

### คำสั่งควบคุมไฟ (สำหรับอุปกรณ์ LED WS2812)

LED แต่ละดวงใช้ตัวแปร 4 ตัวในรูปแบบ `RGBM`

- **R (แดง)**: ค่า `0` ถึง `F`
- **G (เขียว)**: ค่า `0` ถึง `F`
- **B (น้ำเงิน)**: ค่า `0` ถึง `F`
- **M (โหมดกระพริบ)**: เวลาที่ไฟติดภายใน 1 วินาที `0` หมายถึงดับตลอด `1` หมายถึงติด 100ms ต่อวินาที และเพิ่มขึ้นตามลำดับ ค่า `A` ขึ้นไปหมายถึงติดค้าง
- ตัวอย่างเช่น `F00A` หมายถึงไฟแดงติดค้าง และ `FFF5` หมายถึงไฟสีขาวติด 500ms ดับ 500ms ในแต่ละวินาที

#### G. `LED` (กลุ่มแรก 4 ดวง)
- **รูปแบบ**: 20 Bytes
- **ตัวอย่าง**:
- `LED0111100000111110000022222#`: ควบคุมกลุ่มไฟ LED ด้านหน้า

#### H. `LE2` (กลุ่มที่สอง 4 ดวง)
- **รูปแบบ**: 20 Bytes
- **ตัวอย่าง**:
- `LE20111100000111110000022222#`: ควบคุมกลุ่ม LED ชุดที่สอง

#### I. `LE?` (กลุ่มที่ N จำนวน 4 ดวง)
- **คำอธิบาย**: `?` แทนหมายเลขกลุ่ม `N` ใช้ควบคุม LED 4 ดวงของกลุ่มที่ N
- **ตัวอย่าง**:
- `LE50111100000111110000022222#`: หมายถึงกลุ่ม LED ชุดที่ห้า

### คำสั่งกำหนดเองสำหรับอุปกรณ์

#### J. `CMD` (คำสั่งที่ส่งตรงไปยังอุปกรณ์)
- **คำอธิบาย**: คำสั่งนี้จะส่งข้อมูลไปยังฝั่งอุปกรณ์โดยตรง และความหมายของคำสั่งขึ้นอยู่กับผู้พัฒนาอุปกรณ์ ความยาวสูงสุดคือ 16 ตัวอักษร ไม่รวมคำนำหน้าและสัญลักษณ์ปิดท้าย หากสั้นกว่า 16 ตัวอักษรต้องเติมช่องว่างให้ครบ
- **ตัวอย่าง**:
- หากคำสั่งคือ `RESET` แพ็กเก็ตเต็มจะเป็น `CMDRESET          #`

## Bahasa Melayu

Bagi memenuhi lebih banyak keperluan aplikasi, V7RC Protocol terbuka telah disemak semula untuk memberikan keserasian yang lebih baik. Tahap sokongan sebenar mungkin berbeza mengikut papan pembangunan dan versi perisian tegar.

### Peraturan Asas

Disebabkan had saiz paket BLE, setiap arahan dihadkan kepada **20 Bytes atau kurang**.
Format utama yang disokong pada masa ini ialah 20 Bytes, manakala format lama 16 Bytes tidak lagi disokong oleh APP baharu.

- **Format arahan**: 3 aksara pertama ialah kod arahan.
- **Penanda penamat**: Setiap paket berakhir dengan `#`.
- **Peraturan pelengkap**: Medan yang tidak digunakan diisi dengan `0`, manakala arahan `CMD` menggunakan ruang kosong sebagai pelengkap.

### Huraian Arahan Kawalan

#### A. `SRV` (Kawalan PWM Asas)
- **Penerangan**: Arahan kawalan PWM standard dengan panjang keseluruhan 20 Bytes.
- **Contoh**:
- `SRV1500150015001500#`: Menyokong isyarat PWM untuk P0, P1 dan P2. Versi standard Microbit biasanya menyokong tiga keluaran PWM.
- `SRV1500100018002000#`: Menyokong isyarat PWM untuk C1, C2, C3 dan C4.
- **Nota**: Format lama 16 Bytes seperti `SRV150015001500#` tidak lagi disokong oleh APP baharu dan tidak sepatutnya digunakan.

#### B. `SR2` (Kumpulan PWM Kedua)
- **Penerangan**: Jika papan menyediakan lebih daripada empat saluran PWM, arahan ini digunakan untuk mengawal servo C5 hingga C8.
- **Contoh**:
- `SR21500100018002000#`: Sejajar dengan isyarat PWM bagi C5, C6, C7 dan C8.

#### C. `SS8` (PWM Ringkas untuk 8 Saluran)
- **Penerangan**: Setiap nilai PWM diringkaskan kepada nilai heksadesimal `00` hingga `FF`, dengan ketepatan yang lebih rendah. Tukarkan nilainya ke perpuluhan dahulu, kemudian darab 10 untuk mendapatkan nilai PWM.
- **Contoh**:
- `SS896969696969696#`: `96` menjadi 150 dalam perpuluhan, kemudian 150 didarab 10 menjadi 1500.

#### D. `SSD` (Kawalan Peranti Ringkas dengan Butang, Belum Dilaksanakan)
- **Penerangan**: Setiap output PWM menggunakan 3 digit untuk sudut, dan 4 digit terakhir mewakili empat status GPIO menggunakan `0` atau `1`.
- **Contoh**:
- `SSD0901801800001100#`
- C1 ialah 90 darjah, C2 ialah 180 darjah, C3 ialah 180 darjah, dan C4 ialah 0 darjah.
- GPIO0 ialah ON, GPIO1 ialah ON, GPIO2 ialah OFF, dan GPIO3 ialah OFF.

#### E. `SRT` (Mod Tank Berasaskan PWM Asas)
- **Penerangan**: Variasi kepada arahan PWM asas. CH1 dan CH2 akan ditukar di peringkat perisian tegar menjadi isyarat kawalan tank.
- **Contoh**:
- `SRT1500150015001500#`

#### F. `SST` (Mod Tank Berasaskan Kawalan Ringkas, Belum Dilaksanakan)
- **Penerangan**: Variasi kepada arahan kawalan peranti ringkas. CH1 dan CH2 akan ditukar di peringkat perisian tegar menjadi isyarat kawalan tank.
- **Contoh**:
- `SST0901801800001100#`

### Arahan Lampu (untuk LED WS2812)

Setiap LED menggunakan empat pemboleh ubah dalam bentuk `RGBM`:

- **R (Merah)**: Nilai dari `0` hingga `F`.
- **G (Hijau)**: Nilai dari `0` hingga `F`.
- **B (Biru)**: Nilai dari `0` hingga `F`.
- **M (Mod berkelip)**: Tempoh nyalaan bagi setiap saat. `0` bermaksud sentiasa padam, `1` bermaksud menyala 100ms setiap saat, dan seterusnya. Nilai `A` ke atas bermaksud sentiasa menyala.
- Contohnya, `F00A` bermaksud merah menyala berterusan, manakala `FFF5` bermaksud cahaya putih menyala 500ms dan padam 500ms setiap saat.

#### G. `LED` (Kumpulan Pertama 4 LED)
- **Format**: 20 Bytes.
- **Contoh**:
- `LED0111100000111110000022222#`: Mengawal kumpulan LED hadapan.

#### H. `LE2` (Kumpulan Kedua 4 LED)
- **Format**: 20 Bytes.
- **Contoh**:
- `LE20111100000111110000022222#`: Mengawal kumpulan LED kedua.

#### I. `LE?` (Kumpulan LED ke-N, 4 LED)
- **Penerangan**: `?` digantikan dengan nombor kumpulan `N` untuk mengawal kumpulan LED ke-N.
- **Contoh**:
- `LE50111100000111110000022222#`: Mewakili kumpulan LED kelima.

### Arahan Tersuai Peranti

#### J. `CMD` (Arahan Laluan Terus ke Peranti)
- **Penerangan**: Arahan ini menghantar data terus ke bahagian peranti, dan maknanya ditentukan sepenuhnya oleh pembangun peranti. Panjang maksimum arahan ialah 16 aksara, tidak termasuk awalan dan penanda penamat. Jika kandungan lebih pendek daripada 16 aksara, tambah ruang kosong sehingga cukup panjang.
- **Contoh**:
- Jika arahannya ialah `RESET`, paket penuh ialah `CMDRESET          #`.

## Bahasa Indonesia

Untuk mendukung lebih banyak kebutuhan aplikasi, V7RC Protocol terbuka telah diperbarui agar memberikan kompatibilitas yang lebih baik. Tingkat dukungan aktual dapat berbeda tergantung papan pengembangan dan versi firmware yang digunakan.

### Aturan Dasar

Karena paket transmisi BLE memiliki batas ukuran, setiap perintah dibatasi hingga **20 Bytes atau kurang**.
Format utama yang saat ini didukung adalah 20 Bytes, sedangkan format lama 16 Bytes sudah tidak didukung lagi pada APP versi baru.

- **Format perintah**: 3 karakter pertama adalah kode perintah.
- **Penanda akhir**: Setiap paket diakhiri dengan `#`.
- **Aturan padding**: Kolom yang tidak digunakan diisi dengan `0`, sedangkan perintah `CMD` diisi menggunakan spasi.

### Penjelasan Detail Perintah Kontrol

#### A. `SRV` (Kontrol PWM Dasar)
- **Deskripsi**: Perintah PWM standar dengan panjang total 20 Bytes.
- **Contoh**:
- `SRV1500150015001500#`: Mendukung sinyal PWM untuk P0, P1, dan P2. Versi standar Microbit biasanya mendukung tiga keluaran PWM.
- `SRV1500100018002000#`: Mendukung sinyal PWM untuk C1, C2, C3, dan C4.
- **Catatan**: Format lama 16 Bytes seperti `SRV150015001500#` sudah tidak didukung oleh APP versi baru dan sebaiknya tidak digunakan lagi.

#### B. `SR2` (Grup PWM Kedua)
- **Deskripsi**: Jika papan menyediakan lebih dari empat kanal PWM, perintah ini digunakan untuk mengontrol servo C5 sampai C8.
- **Contoh**:
- `SR21500100018002000#`: Sesuai dengan sinyal PWM untuk C5, C6, C7, dan C8.

#### C. `SS8` (PWM Sederhana untuk 8 Kanal)
- **Deskripsi**: Setiap nilai PWM disederhanakan menjadi nilai heksadesimal `00` sampai `FF`, dengan presisi yang lebih rendah. Ubah nilai tersebut ke desimal terlebih dahulu, lalu kalikan 10 untuk mendapatkan nilai PWM.
- **Contoh**:
- `SS896969696969696#`: `96` menjadi 150 dalam desimal, lalu 150 dikalikan 10 menjadi 1500.

#### D. `SSD` (Kontrol Perangkat Sederhana dengan Tombol, Belum Diimplementasikan)
- **Deskripsi**: Setiap output PWM menggunakan 3 digit untuk sudut, dan 4 digit terakhir mewakili empat status GPIO menggunakan `0` atau `1`.
- **Contoh**:
- `SSD0901801800001100#`
- C1 adalah 90 derajat, C2 adalah 180 derajat, C3 adalah 180 derajat, dan C4 adalah 0 derajat.
- GPIO0 adalah ON, GPIO1 adalah ON, GPIO2 adalah OFF, dan GPIO3 adalah OFF.

#### E. `SRT` (Mode Tank Berbasis PWM Dasar)
- **Deskripsi**: Variasi dari perintah PWM dasar. CH1 dan CH2 akan diubah di sisi firmware menjadi sinyal kontrol tank.
- **Contoh**:
- `SRT1500150015001500#`

#### F. `SST` (Mode Tank Berbasis Kontrol Sederhana, Belum Diimplementasikan)
- **Deskripsi**: Variasi dari perintah kontrol perangkat sederhana. CH1 dan CH2 akan diubah di sisi firmware menjadi sinyal kontrol tank.
- **Contoh**:
- `SST0901801800001100#`

### Perintah Lampu (untuk LED WS2812)

Setiap LED menggunakan empat variabel dalam format `RGBM`:

- **R (Merah)**: Nilai dari `0` hingga `F`.
- **G (Hijau)**: Nilai dari `0` hingga `F`.
- **B (Biru)**: Nilai dari `0` hingga `F`.
- **M (Mode kedip)**: Lama lampu menyala per detik. `0` berarti selalu mati, `1` berarti menyala 100ms setiap detik, dan seterusnya. Nilai `A` ke atas berarti selalu menyala.
- Contohnya, `F00A` berarti merah menyala terus, sedangkan `FFF5` berarti lampu putih menyala 500ms dan mati 500ms setiap detik.

#### G. `LED` (Grup Pertama 4 LED)
- **Format**: 20 Bytes.
- **Contoh**:
- `LED0111100000111110000022222#`: Mengontrol grup LED depan.

#### H. `LE2` (Grup Kedua 4 LED)
- **Format**: 20 Bytes.
- **Contoh**:
- `LE20111100000111110000022222#`: Mengontrol grup LED kedua.

#### I. `LE?` (Grup ke-N, 4 LED)
- **Deskripsi**: `?` diganti dengan nomor grup `N` untuk mengontrol grup ke-N yang berisi empat LED.
- **Contoh**:
- `LE50111100000111110000022222#`: Mewakili grup LED kelima.

### Perintah Kustom Perangkat

#### J. `CMD` (Perintah Pass-Through ke Perangkat)
- **Deskripsi**: Perintah ini meneruskan data langsung ke sisi perangkat, dan maknanya sepenuhnya ditentukan oleh pengembang perangkat. Panjang maksimum perintah adalah 16 karakter, tidak termasuk prefiks dan penanda akhir. Jika isi perintah kurang dari 16 karakter, tambahkan spasi sebagai padding.
- **Contoh**:
- Jika perintahnya adalah `RESET`, paket lengkapnya adalah `CMDRESET          #`.
