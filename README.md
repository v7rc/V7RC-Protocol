# V7RC-Protocol

## Before you start:
This repository is used to define V7RC protocol which is used in development for V7RC APP and related MCU dev boards. Normally V7RC team will plan and release an new features on APP, then Makers could reference this document to develop their own dev boards. If you have any question to read the document, please leave a message to us.

## V7RC Protocol (Implement on V7RC APP Already)

### Basic concept
Basically, the design concept of V7RC protocol is based on the payload of BLE. Since we need to use WIFI and BLE interface to transmit data to MCU dev board, the V7RC protocol must be compactiable with most WIFI and BLE transaction. The length of each V7RC payload is 20. Yes! The V7RC APP will send V7RC payloads and receive V7RC payloads and those payload must follow V7RC Protocol. If you read and follow the spec, you would be easy to use V7RC APP and use it to remote control your devices.

About the format of each V7RC payload:

Here is an example of V7RC Payload, i.e.

`SRV1500150015001500#`

1. Length: must be 20 bytes. (Maybe it would be extend to unlimited in future)
2. First 3 bytes is COMMAND string: from the example. you can see 'SRV' at first 3 bytes and that is a COMMAND and meant to CONTROL PWM motor channels.
3. The END signature is '#'. From the example, you can see the '#' in the end of payload. It is easy for you to specify each of paylods.
4. There are 16 bytes for data bytes. most of them are ASCII data, some command use bunary data (just like SS8 command).




## Plan to develop in next phase.
