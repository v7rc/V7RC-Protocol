# V7RC-Protocol

[Language Index](README.md)

## Before You Start
This repository defines the V7RC Protocol used by the V7RC APP and related MCU development boards. In most cases, the V7RC team plans and releases new APP features first, and makers can then use this document to build their own compatible boards and devices. If you have any questions while reading the document, please contact us.

## About the V7RC Protocol
The V7RC Protocol is designed around the BLE payload concept. Because data may be transmitted to MCU development boards through both WIFI and BLE, the protocol is designed to stay compatible with common WIFI and BLE transmission scenarios. Each V7RC payload is fixed at 20 bytes. The V7RC APP sends and receives packets in this format, so following this specification makes it much easier to integrate your device with the V7RC APP for remote control.

## Payload Format
Example:

`SRV1500150015001500#`

1. The payload length must be 20 bytes, although it may be extended in the future.
2. The first 3 bytes are the command string. In this example, `SRV` means PWM motor channel control.
3. The payload always ends with `#`, which makes each packet easy to identify.
4. The middle 16 bytes are the data field. Most commands use ASCII data, while some commands such as `SS8` use compact binary-like values.

## Common Commands
- `SRV`
- `SRT`
- `SS8`

## Next Phase
For the full protocol description, see [protocol.en.md](protocol.en.md).

