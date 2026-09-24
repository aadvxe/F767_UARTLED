# F767_UARTLED (UART DMA Controlled LED System)

An embedded STM32CubeIDE firmware project for the **NUCLEO-F767ZI** featuring an interactive UART DMA-driven console and state machine to control onboard LEDs.

---

## 🎯 Project Overview

This project implements non-blocking serial communication using **USART3** combined with **DMA (Direct Memory Access)** for both reception and transmission on the STM32F767ZI microcontroller. An ASCII command menu allows users or automated GUI tools to remotely switch, toggle, and configure multi-speed blinking patterns on the onboard LED (`PB7` / `LD2`).

- **MCU:** STM32F767ZIT6 (ARM Cortex-M7 @ 216 MHz)
- **Board:** NUCLEO-F767ZI
- **Serial Interface:** USART3 (ST-LINK Virtual COM Port, 115200 Baud, 8N1)
- **DMA Channels:** `DMA1_Stream1` (USART3_RX) & `DMA1_Stream3` (USART3_TX)
- **Controlled Hardware:** User LED `LD2` (`PB7`, Blue LED)

---

## 📟 Interactive Command Menu

The firmware transmits an ASCII menu over UART and parses single-character commands terminated by a carriage return (`\r`):

| Command Key | Function | Description |
| :---: | :--- | :--- |
| **`1`** | **LED ON** | Sets `PB7` output HIGH (turns on Blue LED). |
| **`2`** | **LED OFF** | Sets `PB7` output LOW (turns off Blue LED). |
| **`3`** | **Slow Blink** | Toggles LED periodically every **2000 ms** (2.0 s). |
| **`4`** | **Medium Blink** | Toggles LED periodically every **1000 ms** (1.0 s). |
| **`5`** | **Fast Blink** | Toggles LED periodically every **500 ms** (0.5 s). |
| **`6`** | **Stop Blink** | Disables active blinking mode. |
| **`7`** | **Read Status** | Responds with current state: `LED Status <0 or 1>\r`. |

---

## 💻 Companion Software

This firmware is designed to interface seamlessly with:
- **[serialGUI](file:///c:/Users/Rangga/Documents/Old%20Project%20Archive/serialGUI):** A Python Tkinter desktop GUI that sends serial control signals with automatic button debouncing.
- **Any Serial Terminal:** PuTTY, Tera Term, minicom, or Arduino Serial Monitor configured at:
  - **Baud Rate:** `115200`
  - **Data Bits:** `8`, **Stop Bits:** `1`, **Parity:** `None`
  - **Flow Control:** `None`

---

## 📁 Folder Structure

```
F767_UARTLED/
├── Core/
│   ├── Inc/
│   │   ├── main.h              # Pin & peripheral declarations
│   │   ├── stm32f7xx_hal_conf.h
│   │   └── stm32f7xx_it.h
│   └── Src/
│       ├── main.c              # UART DMA handlers, menu loop, state machine
│       ├── stm32f7xx_hal_msp.c # Low-level GPIO, DMA & USART3 init
│       ├── stm32f7xx_it.c      # DMA & USART3 ISRs
│       └── system_stm32f7xx.c
├── Drivers/                    # STM32F7xx HAL & CMSIS
├── F767_UARTLED.ioc            # STM32CubeMX configuration
└── STM32F767ZITX_FLASH.ld      # Linker script
```

---

## 🛠️ Building & Flashing

1. Open **STM32CubeIDE**.
2. Select **File > Open Projects from File System...** and open `F767_UARTLED`.
3. Connect the NUCLEO-F767ZI via USB.
4. Build the project (`Ctrl+B`) and flash (`Ctrl+F11`).
5. Open your preferred serial terminal at 115200 baud to interact with the device.
