# V7RC IO Command Protocol

[Language Index](protocol.md)

The open V7RC Protocol has been revised to support more application scenarios and improve compatibility. Actual support may still vary depending on the development board and firmware version.

## Basic Rules

Because BLE transmission packets have a limited size, each command is restricted to **20 bytes or less**.
The current main format is 20 bytes, and the older 16-byte format is no longer supported in the newer APP.

- **Command format**: The first 3 characters are the command code.
- **Ending marker**: Every packet ends with `#`.
- **Padding rule**: Unused fields are padded with `0`, while the `CMD` instruction uses spaces for padding.

## Detailed Control Commands

### `SRV` (Basic PWM Control)
- **Description**: Standard PWM control command with a total length of 20 bytes.
- **Examples**:
- `SRV1500150015001500#`: Supports PWM signals for P0, P1, and P2. The standard Microbit version typically supports three PWM outputs.
- `SRV1500100018002000#`: Supports PWM signals for C1, C2, C3, and C4.
- **Note**: The old 16-byte version such as `SRV150015001500#` is no longer supported by the newer APP and should not be used.

### `SR2` (Second PWM Group)
- **Description**: If the board provides more than four PWM channels, this command controls servo channels C5 to C8.
- **Example**:
- `SR21500100018002000#`: Corresponds to PWM signals for C5, C6, C7, and C8.

### `SS8` (Simplified PWM for 8 Channels)
- **Description**: Each PWM value is simplified into a hexadecimal value from `00` to `FF`, with lower precision. Convert the value to decimal first, then multiply it by 10 to get the PWM value.
- **Example**:
- `SS896969696969696#`: `96` becomes decimal 150, and 150 multiplied by 10 becomes 1500.

### `SSD` (Simplified Device Control with Buttons, Not Implemented)
- **Description**: Each PWM output uses 3 digits for angle control, and the last 4 digits represent four GPIO states using `0` or `1`.
- **Example**:
- `SSD0901801800001100#`
- C1 is 90 degrees, C2 is 180 degrees, C3 is 180 degrees, and C4 is 0 degrees.
- GPIO0 is ON, GPIO1 is ON, GPIO2 is OFF, and GPIO3 is OFF.

### `SRT` (Tank Mode Based on Basic PWM)
- **Description**: A variation of the basic PWM command. CH1 and CH2 are converted in firmware into tank-control signals.
- **Example**:
- `SRT1500150015001500#`

### `SST` (Tank Mode Based on Simplified Device Control, Not Implemented)
- **Description**: A variation of the simplified device control command. CH1 and CH2 are converted in firmware into tank-control signals.
- **Example**:
- `SST0901801800001100#`

## Lighting Commands (for WS2812 LED Devices)

Each LED uses four variables in the form `RGBM`:

- **R (Red)**: A value from `0` to `F`.
- **G (Green)**: A value from `0` to `F`.
- **B (Blue)**: A value from `0` to `F`.
- **M (Blink mode)**: The lighting time per second. `0` means always off, `1` means on for 100ms each second, and so on. Values `A` and above mean always on.
- For example, `F00A` means solid red, and `FFF5` means white light on for 500ms and off for 500ms every second.

### `LED` (First Group of Four LEDs)
- **Format**: 20 bytes.
- **Example**:
- `LED0111100000111110000022222#`: Controls the front LED group.

### `LE2` (Second Group of Four LEDs)
- **Format**: 20 bytes.
- **Example**:
- `LE20111100000111110000022222#`: Controls the second LED group.

### `LE?` (Nth Group of Four LEDs)
- **Description**: `?` is replaced by the group number `N` to control the Nth group of four LEDs.
- **Example**:
- `LE50111100000111110000022222#`: Represents the fifth LED group.

## Custom Device Commands

### `CMD` (Pass-Through Command for the Device)
- **Description**: This command passes data directly to the device side, and its meaning is fully defined by the device developer. The maximum command length is 16 characters, not including the prefix and ending marker. If the content is shorter than 16 characters, pad it with spaces.
- **Example**:
- If the command is `RESET`, the full packet is `CMDRESET          #`.

