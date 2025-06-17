# UART DMA Echo Example with STM32F446RE

## Description

This project demonstrates how to **receive data via UART using DMA** on the STM32F446RE microcontroller and **echo it back** using interrupt-driven logic. It uses the **UART IDLE line interrupt** to detect when the end of data reception occurs and immediately echoes the received data.

---

## Hardware Used

- **Board**: STM32 Nucleo-F446RE  
- **UART Interface**: USART2 (connected to ST-Link Virtual COM port)  
- **LED**: PA5 (initialized but not used in this example)  
- **Toolchain**: STM32CubeIDE with STM32 HAL Drivers

---

## Peripherals Configuration

### UART (USART2)
- **Baud Rate**: `115200`
- **Word Length**: `8 Bits`
- **Stop Bits**: `1`
- **Parity**: `None`
- **Mode**: `TX/RX`
- **Flow Control**: `None`

### DMA
- DMA1_Stream5 is used for USART2 RX
- Continuously stores incoming UART data into a 64-byte circular buffer

### Interrupts
- **DMA1_Stream5_IRQn**: for DMA RX
- **USART2_IRQn (UART_IT_IDLE)**: triggers when the UART line goes idle (indicating the end of transmission)

---

## How It Works

1. **Initialization**:
   - The system clock and all peripherals are initialized.
   - UART2 is configured to receive data using DMA.
   - The IDLE line interrupt is enabled to detect the end of a data frame.

2. **Reception Buffer**:
   - A circular buffer (`rxBuffer[64]`) is used to store incoming data.
   - DMA continuously fills this buffer without CPU intervention.

3. **Data Processing**:
   - When the UART line goes idle, the position of the DMA pointer is checked.
   - Data between the last known position and the new DMA position is considered new.
   - This new data is echoed back to the sender using `HAL_UART_Transmit`.

4. **Main Loop**:
   - The main loop is empty since all data handling is performed inside the interrupt.

---

## Key Files

| File             | Description                                      |
|------------------|--------------------------------------------------|
| `main.c`         | Main application code                            |
| `stm32f4xx_it.c` | Interrupt handlers (must handle UART IDLE line)  |
| `usart.c`        | UART2 initialization and configuration           |
| `dma.c`          | DMA setup for UART RX                            |
| `gpio.c`         | Initializes GPIO pin PA5                         |

---
## Author

 Deva Nanda S
