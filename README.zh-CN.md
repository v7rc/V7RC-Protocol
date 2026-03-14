# V7RC-Protocol

[语言索引](README.md)

## 开始之前
此仓库用于定义 V7RC Protocol，供 V7RC APP 与相关 MCU 开发板开发时参考。通常 V7RC 团队会先规划并在 APP 端发布新功能，之后 Makers 可以根据本文档开发自己的控制板或设备。如果你在阅读文档时有任何问题，欢迎与我们联系。

## 关于 V7RC Protocol
V7RC Protocol 的设计核心基于 BLE Payload。由于需要通过 WIFI 与 BLE 将数据传输到 MCU 开发板，因此协议必须尽量兼容常见的 WIFI 与 BLE 传输场景。每个 V7RC Payload 的长度固定为 20 Bytes。V7RC APP 会按照此格式发送与接收封包，只要依照此规范实现，就能更轻松地将设备整合到 V7RC APP 中进行远程控制。

## Payload 格式
示例：

`SRV1500150015001500#`

1. 长度必须为 20 Bytes，未来可能扩充。
2. 前 3 Bytes 为命令字符串，例如 `SRV` 表示控制 PWM 电机通道。
3. 结尾固定为 `#`，便于识别每一笔 Payload。
4. 中间 16 Bytes 为数据区，大多数命令使用 ASCII 数据，少数命令会使用类似二进制的紧凑数据，例如 `SS8`。

## 常见命令
- `SRV`
- `SRT`
- `SS8`

## 下一阶段规划
完整协议说明请参考 [protocol.zh-CN.md](protocol.zh-CN.md)。

