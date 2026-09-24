# Aegis Development Journal

## Stage 1 — UART Debug & Interrupt Bring-Up

**Status:** Complete

### Objective
Establish a serial debugging interface between the STM32G474RE and host PC and verify basic interrupt-driven input handling.

### Implemented
- Configured the NUCLEO-G474RE COM interface at 115200 baud.
- Verified UART output through the ST-LINK Virtual COM Port.
- Added serial startup and heartbeat logging using `printf()`.
- Used `HAL_GetTick()` to report system uptime.
- Added a persistent heartbeat counter to verify continuous firmware execution.
- Configured the onboard USER button using an external interrupt (EXTI).
- Overrode the BSP button callback to signal button events using a `volatile` flag.
- Processed button events in the main application loop rather than performing UART output inside the interrupt callback.

### Verified
Example UART output:

    [AEGIS] Heartbeat: 7 | Uptime: 12024 ms
    [INPUT] USER button pressed
    [AEGIS] Heartbeat: 8 | Uptime: 14030 ms

This verified communication from the STM32 through UART/ST-LINK to the host PC and confirmed that external button interrupts are handled successfully.

### Lessons Learned
- Embedded firmware normally executes continuously rather than terminating like a desktop application.
- UART provides a useful observability/debug channel for firmware.
- Interrupt handlers should remain short; heavier processing can be deferred to the main application.
- `volatile` is required for state that may be modified asynchronously by an interrupt.
- Long blocking calls such as `HAL_Delay()` can delay application-level processing even though hardware interrupts can still occur.

### Next
Stage 2: External peripheral communication over I²C using an SSD1306 OLED.