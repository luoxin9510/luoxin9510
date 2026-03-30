---
name: asic-ip-spec
description: >
  Generate a complete, professional ASIC IP specification document in Markdown,
  modelled on STMicroelectronics TRM structure (RM0090 style).
  Use when the user wants to write, define, or generate an IP spec, hardware block spec,
  RTL spec, or IP documentation for an ASIC or SoC design.
  Triggers on: "write an IP spec", "generate IP spec", "ASIC spec", "hardware spec",
  "RTL spec", "SoC IP documentation", "/asic-ip-spec".
---

# ASIC IP Spec Generator

Guide the user through gathering all required information for a hardware IP block, then
produce a complete, well-structured IP specification document in Markdown.

Document structure is modelled on STMicroelectronics technical reference manuals
(e.g. RM0090 for STM32F4xx) — the industry reference for IP peripheral documentation.

## ST TRM Chapter Pattern (reference)

Every ST peripheral chapter follows this consistent structure:

```
X.1  Introduction          — what the IP is, which SoC variants include it
X.2  Main features         — bullet list of capabilities
X.3  Functional description — subdivided by operating mode / sub-block
X.4  Interrupts            — sources, flags, enable bits (DEDICATED section)
X.5  Low-power modes       — IP behavior in Sleep / Stop / Standby
X.6  Register description  — one subsection per register + register map table
```

This skill generates a spec following this proven structure.

---

## Workflow

Make a todo list and work through each step one at a time.

### 1. Gather IP Information

Use AskUserQuestion to collect the following. Batch related questions — don't ask one at a time.

**Batch 1 — Identity:**
- IP block name (e.g. `uart_ctrl`, `dma_engine`, `axi_bridge`)
- One-line functional description
- Which SoC / chip variants include this IP (e.g. "all variants", "only high-end SKU")
- Target process node (e.g. TSMC 7nm, GF 22nm, generic)
- Document version, author, date

**Batch 2 — Architecture:**
- Key features (user provides bullet list)
- Top-level interfaces: bus protocol (AXI4, APB3, AHB, custom), data I/O, clocks, resets
- Number of clock domains and reset domains
- Block diagram (ask user to describe or provide ASCII)

**Batch 3 — Functional Modes:**
- What are the main operating modes? (e.g. TX mode, RX mode, loopback, DMA mode)
- Any special sub-modes (burst, FIFO, synchronous/asynchronous, half-duplex)?
- Baud rate / frequency generation mechanism (if applicable)
- Hardware flow control (if applicable)

**Batch 4 — Interrupts & Power:**
- Interrupt sources (list each condition that generates an interrupt)
- Are interrupts level or edge? Active high or low?
- Low-power mode behavior: what happens in Sleep / Stop / Standby?
- Can this IP wake the system from a low-power mode?

**Batch 5 — Registers:**
- Number of registers and address space size
- List of register names (user can provide rough list; Claude will structure them)

If user says "just generate it", use reasonable defaults and mark assumptions clearly as:
`> **Assumption:** ...`

---

### 2. Generate the Spec Document

Write the full Markdown document using the template below. Save to:
`./docs/<ip-name>/<ip-name>_spec.md`

For sections where data is missing, add: `> _TODO: [what is needed]_`

---

## Document Template

````markdown
# <IP_NAME> IP Specification

| Field        | Value                          |
|--------------|-------------------------------|
| Document     | <IP_NAME> IP Specification    |
| Version      | 0.1                           |
| Status       | Draft                         |
| Author       | <author>                      |
| Date         | <YYYY-MM-DD>                  |
| Applies to   | <SoC name / all variants>     |
| Target node  | <process node>                |

---

## Table of Contents

1. [Introduction](#1-introduction)
2. [Main Features](#2-main-features)
3. [Functional Description](#3-functional-description)
4. [Interrupts](#4-interrupts)
5. [Low-Power Modes](#5-low-power-modes)
6. [Register Description](#6-register-description)
7. [Interface Description](#7-interface-description)
8. [Clocking](#8-clocking)
9. [Reset](#9-reset)
10. [Timing Requirements](#10-timing-requirements)
11. [Verification Plan](#11-verification-plan)
12. [Known Limitations](#12-known-limitations)
13. [Revision History](#13-revision-history)

---

## 1. Introduction

<2–4 sentences: what this IP does, its role in the SoC, and which device variants include it.>

The <IP_NAME> is available on: <list variants, e.g. "all SKUs" or "high-performance variant only">.

---

## 2. Main Features

- <Feature 1, e.g. "Full-duplex asynchronous communication">
- <Feature 2, e.g. "Programmable baud rate up to X Mbps">
- <Feature 3, e.g. "Hardware flow control (CTS/RTS)">
- <Feature 4, e.g. "DMA-capable TX and RX channels">
- <Feature 5, e.g. "FIFO depth: 16 entries × 32-bit">
- <Feature 6, e.g. "Multiprocessor communication support">
- <Feature 7, e.g. "Single-wire half-duplex mode">
- <Feature 8, e.g. "Wakeup from Stop mode via received character">

---

## 3. Functional Description

### 3.1 Block Diagram

```
                  ┌──────────────────────────────────────────┐
                  │               <IP_NAME>                  │
                  │                                          │
  APB/AXI  ──►   │  ┌──────────┐    ┌──────────────────┐   │
                  │  │ Reg File │───►│    Datapath /     │   │──► TX_DATA
  CLK      ──►   │  └──────────┘    │    State Machine  │   │
  RSTn     ──►   │                  └──────────┬────────┘   │◄── RX_DATA
                  │  ┌──────────┐              │            │
                  │  │ Int Ctrl │◄─────────────┘            │──► IRQ
                  │  └──────────┘                           │
                  └──────────────────────────────────────────┘
```

> _TODO: Replace with actual block diagram._

### 3.2 <Operating Mode 1, e.g. Transmitter>

<Describe the transmit path, data flow, enabling sequence, and relevant signals.>

**Enabling sequence:**
1. Configure word length, baud rate, parity in control registers
2. Set ENABLE bit in CTRL register
3. Write data to TX_DATA register
4. Poll/interrupt on TX_EMPTY flag

**Timing diagram (conceptual):**
```
CLK   ___/‾\_/‾\_/‾\_/‾\_/‾\_/‾\_/‾\_/‾\
TX    ‾‾‾\___[D0][D1][D2][D3][D4][D5][D6][D7]‾‾‾
```

### 3.3 <Operating Mode 2, e.g. Receiver>

<Describe the receive path.>

### 3.4 <Baud Rate / Clock Divider Generation>

The baud rate is derived from the peripheral clock (PCLK) using:

```
Baud Rate = PCLK / (16 × USARTDIV)
```

Where `USARTDIV` is the value programmed in the BRR register.

| PCLK (MHz) | Target Baud Rate | BRR Value | Actual Rate  | Error |
|------------|-----------------|-----------|-------------|-------|
| 50         | 115,200          | 27.127    | 115,207     | 0.006%|
| 50         | 1,000,000        | 3.125     | 1,000,000   | 0%    |

### 3.5 DMA Mode

When DMA mode is enabled (CTRL.DMA_EN = 1):
- TX FIFO threshold asserts DMA request to DMA controller
- RX FIFO threshold asserts DMA request to DMA controller
- CPU is not involved in data movement

### 3.6 Hardware Flow Control

<If applicable: describe CTS/RTS or equivalent handshake signals and behavior.>
> _TODO: Describe flow control mechanism or mark N/A._

### 3.7 FIFO Operation

| Parameter    | Value           |
|--------------|----------------|
| TX FIFO depth| <N> entries × <W>-bit |
| RX FIFO depth| <N> entries × <W>-bit |
| TX threshold | Programmable: 1/4, 1/2, 3/4 full |
| RX threshold | Programmable: 1/4, 1/2, 3/4 full |

---

## 4. Interrupts

All interrupt flags are in the INT_STAT register. Each source has a corresponding enable bit in INT_EN. The combined interrupt output (`irq`) is asserted when any enabled flag is set.

| Source | Flag Bit | Enable Bit | Trigger Condition | Type |
|--------|----------|-----------|-------------------|------|
| TX Empty | INT_STAT[0] | INT_EN[0] | TX FIFO below threshold | Level |
| RX Not Empty | INT_STAT[1] | INT_EN[1] | RX FIFO above threshold | Level |
| TX Complete | INT_STAT[2] | INT_EN[2] | Last byte shifted out | Level |
| Overrun Error | INT_STAT[3] | INT_EN[3] | RX overflow | Level |
| Framing Error | INT_STAT[4] | INT_EN[4] | Invalid stop bit | Level |
| Parity Error | INT_STAT[5] | INT_EN[5] | Parity mismatch | Level |

**Clearing interrupts:** Write 1 to the corresponding INT_STAT bit (W1C).

**Interrupt priority:** All sources share a single `irq` output line. The host interrupt controller (NVIC/GIC) assigns priority.

---

## 5. Low-Power Modes

| Mode | IP State | Wake-up Capable | Notes |
|------|----------|-----------------|-------|
| Sleep | Fully operational | Yes — any enabled interrupt | CPU halted, peripheral clocks active |
| Stop | Clock gated, state retained | Yes — if LP_WAKEUP_EN = 1 | RX pin monitored by always-on logic |
| Standby | Power removed, state lost | No | Registers reset on exit |

### 5.1 Wake-up from Stop Mode

If `CTRL.LP_WAKEUP_EN` is set, the IP can detect a start bit on the RX pin and assert a wake-up event to the power management unit, restoring clocks before the first character is received.

> _TODO: Confirm wake-up latency and whether a start/stop bit is missed during clock restoration._

---

## 6. Register Description

Base address: `0x0000_0000` (relative to IP base, assign SoC base at integration time)

### 6.1 CTRL — Control Register

**Offset:** `0x00` | **Access:** RW | **Reset:** `0x0000_0000`

| Bits  | Field       | Access | Reset | Description                                 |
|-------|-------------|--------|-------|---------------------------------------------|
| 31:16 | RESERVED    | —      | 0     | Reserved, must write 0                      |
| 15:12 | BAUD_DIV    | RW     | 0x0   | Baud rate integer divider (see Section 3.4) |
| 11:8  | MODE        | RW     | 0x0   | 0=UART, 1=Sync, 2=Half-duplex, 3=Loopback  |
| 7     | LP_WAKEUP_EN| RW     | 0     | 1 = Enable wake-up from Stop mode           |
| 6     | DMA_EN      | RW     | 0     | 1 = Enable DMA mode for TX and RX           |
| 5     | FLOW_CTRL   | RW     | 0     | 1 = Enable CTS/RTS hardware flow control    |
| 4     | PARITY_EN   | RW     | 0     | 1 = Enable parity                           |
| 3     | PARITY_SEL  | RW     | 0     | 0 = Even, 1 = Odd                           |
| 2     | WORD_LEN    | RW     | 0     | 0 = 8-bit, 1 = 9-bit word length            |
| 1     | TX_EN       | RW     | 0     | 1 = Transmitter enable                      |
| 0     | RX_EN       | RW     | 0     | 1 = Receiver enable                         |

### 6.2 STATUS — Status Register

**Offset:** `0x04` | **Access:** RO | **Reset:** `0x0000_0040`

| Bits  | Field       | Access | Reset | Description                                  |
|-------|-------------|--------|-------|----------------------------------------------|
| 31:8  | RESERVED    | —      | 0     | Reserved                                     |
| 7     | TX_FULL     | RO     | 0     | 1 = TX FIFO full                             |
| 6     | TX_EMPTY    | RO     | 1     | 1 = TX FIFO empty (default after reset)      |
| 5     | RX_FULL     | RO     | 0     | 1 = RX FIFO full                             |
| 4     | RX_EMPTY    | RO     | 0     | 1 = RX FIFO empty                            |
| 3     | TX_BUSY     | RO     | 0     | 1 = Transmitter actively shifting data       |
| 2     | OVERRUN     | RO     | 0     | 1 = RX FIFO overrun occurred                 |
| 1     | FRAMING_ERR | RO     | 0     | 1 = Framing error on last received character |
| 0     | PARITY_ERR  | RO     | 0     | 1 = Parity error on last received character  |

### 6.3 INT_EN — Interrupt Enable Register

**Offset:** `0x08` | **Access:** RW | **Reset:** `0x0000_0000`

| Bits  | Field       | Access | Reset | Description                       |
|-------|-------------|--------|-------|-----------------------------------|
| 31:6  | RESERVED    | —      | 0     | Reserved                          |
| 5     | PARITY_ERR_EN | RW   | 0     | 1 = Enable parity error interrupt |
| 4     | FRAME_ERR_EN| RW     | 0     | 1 = Enable framing error interrupt|
| 3     | OVERRUN_EN  | RW     | 0     | 1 = Enable overrun interrupt      |
| 2     | TX_CPLT_EN  | RW     | 0     | 1 = Enable TX complete interrupt  |
| 1     | RX_NE_EN    | RW     | 0     | 1 = Enable RX not empty interrupt |
| 0     | TX_E_EN     | RW     | 0     | 1 = Enable TX empty interrupt     |

### 6.4 INT_STAT — Interrupt Status Register

**Offset:** `0x0C` | **Access:** W1C | **Reset:** `0x0000_0000`

| Bits  | Field       | Access | Reset | Description                              |
|-------|-------------|--------|-------|------------------------------------------|
| 31:6  | RESERVED    | —      | 0     | Reserved                                 |
| 5     | PARITY_ERR  | W1C    | 0     | Parity error — write 1 to clear          |
| 4     | FRAME_ERR   | W1C    | 0     | Framing error — write 1 to clear         |
| 3     | OVERRUN     | W1C    | 0     | RX overrun — write 1 to clear            |
| 2     | TX_CPLT     | W1C    | 0     | TX shift register empty — write 1 to clear |
| 1     | RX_NE       | W1C    | 0     | RX FIFO not empty — write 1 to clear     |
| 0     | TX_E        | W1C    | 0     | TX FIFO empty — write 1 to clear         |

### 6.5 TX_DATA — Transmit Data Register

**Offset:** `0x10` | **Access:** WO | **Reset:** `0x0000_0000`

| Bits  | Field  | Access | Reset | Description                    |
|-------|--------|--------|-------|--------------------------------|
| 31:9  | RESERVED | —    | 0     | Reserved                       |
| 8:0   | TXD    | WO     | 0     | Write to push data into TX FIFO (bits [7:0] for 8-bit mode, [8:0] for 9-bit) |

### 6.6 RX_DATA — Receive Data Register

**Offset:** `0x14` | **Access:** RO | **Reset:** `0x0000_0000`

| Bits  | Field  | Access | Reset | Description                          |
|-------|--------|--------|-------|--------------------------------------|
| 31:9  | RESERVED | —    | 0     | Reserved                             |
| 8:0   | RXD    | RO     | 0     | Read to pop data from RX FIFO (8 or 9 bits) |

### 6.7 BRR — Baud Rate Register

**Offset:** `0x18` | **Access:** RW | **Reset:** `0x0000_0000`

| Bits  | Field       | Access | Reset | Description                                  |
|-------|-------------|--------|-------|----------------------------------------------|
| 31:16 | RESERVED    | —      | 0     | Reserved                                     |
| 15:4  | BRR_INT     | RW     | 0     | Integer part of USARTDIV                     |
| 3:0   | BRR_FRAC    | RW     | 0     | Fractional part of USARTDIV (÷16)            |

### 6.8 Register Map

| Offset | Register  | Access | Reset Value   | Description             |
|--------|-----------|--------|---------------|-------------------------|
| `0x00` | CTRL      | RW     | `0x0000_0000` | Control                 |
| `0x04` | STATUS    | RO     | `0x0000_0040` | Status                  |
| `0x08` | INT_EN    | RW     | `0x0000_0000` | Interrupt enable        |
| `0x0C` | INT_STAT  | W1C    | `0x0000_0000` | Interrupt status        |
| `0x10` | TX_DATA   | WO     | `0x0000_0000` | Transmit data           |
| `0x14` | RX_DATA   | RO     | `0x0000_0000` | Receive data            |
| `0x18` | BRR       | RW     | `0x0000_0000` | Baud rate               |
| `0x1C`–`0xFF` | — | —    | —             | Reserved                |

---

## 7. Interface Description

### 7.1 Port List

| Port Name   | Dir    | Width | Clock Domain | Description                         |
|-------------|--------|-------|--------------|-------------------------------------|
| `clk`       | input  | 1     | —            | Primary clock (PCLK)                |
| `rst_n`     | input  | 1     | —            | Active-low synchronous reset        |
| `psel`      | input  | 1     | clk          | APB select                          |
| `penable`   | input  | 1     | clk          | APB enable phase                    |
| `pwrite`    | input  | 1     | clk          | APB write strobe                    |
| `paddr`     | input  | 8     | clk          | APB address [7:0]                   |
| `pwdata`    | input  | 32    | clk          | APB write data                      |
| `prdata`    | output | 32    | clk          | APB read data                       |
| `pready`    | output | 1     | clk          | APB ready (can insert wait states)  |
| `pslverr`   | output | 1     | clk          | APB error response                  |
| `tx`        | output | 1     | clk          | Serial transmit output              |
| `rx`        | input  | 1     | clk          | Serial receive input                |
| `cts_n`     | input  | 1     | clk          | CTS flow control (active low)       |
| `rts_n`     | output | 1     | clk          | RTS flow control (active low)       |
| `dma_tx_req`| output | 1     | clk          | DMA TX request                      |
| `dma_rx_req`| output | 1     | clk          | DMA RX request                      |
| `irq`       | output | 1     | clk          | Interrupt (level, active-high)      |

> _TODO: Add any IP-specific ports not listed above._

### 7.2 Interface Protocols

| Interface | Protocol | Version | Data Width | Notes               |
|-----------|----------|---------|-----------|---------------------|
| Config    | APB3     | v2.0    | 32-bit    | Slave               |
| DMA       | Sideband | —       | 1-bit req | TX and RX channels  |

---

## 8. Clocking

| Clock  | Freq (max) | Source     | Description                          |
|--------|-----------|-----------|--------------------------------------|
| `clk`  | 500 MHz    | PCLK      | All synchronous logic                |

### 8.1 Clock Enable / Gating

The IP clock is gated by the SoC clock controller when the IP is idle and no interrupt is pending. Clock gating is transparent to software — re-enable by any register access.

### 8.2 Clock Domain Crossings

> _TODO: List CDCs or state "single clock domain — no CDCs"._

---

## 9. Reset

| Signal  | Polarity   | Type        | Scope   |
|---------|-----------|-------------|---------|
| `rst_n` | Active low | Synchronous | Full IP |

**On reset:**
- All registers return to reset values (Section 6.8)
- TX and RX FIFOs flushed
- All outputs driven to inactive state
- `tx` output held high (idle/mark state)
- Minimum reset assertion: **2 clock cycles**

---

## 10. Timing Requirements

| Parameter           | Min  | Typ | Max  | Unit | Conditions            |
|--------------------|------|-----|------|------|-----------------------|
| Clock period        | 2.0  | —   | —    | ns   | 500 MHz max           |
| Input setup (rx/cts)| 0.2  | —   | —    | ns   | To clk rising edge    |
| Input hold (rx/cts) | 0.1  | —   | —    | ns   | From clk rising edge  |
| Output valid (tx)   | —    | —   | 1.5  | ns   | From clk rising edge  |
| Reset pulse width   | 4.0  | —   | —    | ns   | Min 2 cycles @ 500MHz |

> _TODO: Populate with actual STA results._

---

## 11. Verification Plan

### 11.1 Testbench Architecture

| Component       | Type          | Description                             |
|-----------------|---------------|-----------------------------------------|
| APB VIP         | UVM Agent     | Drive register accesses                 |
| Serial BFM      | UVM Agent     | Drive/monitor TX/RX serial data         |
| Reference Model | SystemVerilog | Golden model for output prediction      |
| Scoreboard      | UVM           | Compare DUT vs reference                |
| Coverage        | UVM           | Functional and code coverage            |

### 11.2 Test Plan

| Test Name           | Category   | Description                                       |
|---------------------|-----------|---------------------------------------------------|
| `tc_reset`          | Basic      | Reset values, register accessibility             |
| `tc_reg_rw`         | Register   | All RW fields, reserved bits ignore writes       |
| `tc_tx_basic`       | Functional | TX in 8-bit UART mode                            |
| `tc_rx_basic`       | Functional | RX in 8-bit UART mode                            |
| `tc_baud_rates`     | Functional | Multiple baud rates, accuracy check              |
| `tc_fifo_full`      | Functional | TX/RX FIFO fill and drain                        |
| `tc_dma_mode`       | Functional | DMA TX and RX channels                           |
| `tc_flow_ctrl`      | Functional | CTS/RTS hardware flow control                    |
| `tc_all_modes`      | Functional | UART / Sync / Half-duplex / Loopback modes       |
| `tc_interrupts`     | Interrupt  | All interrupt sources, enable/mask, W1C          |
| `tc_lp_wakeup`      | Power      | Wake from Stop mode via RX start bit             |
| `tc_overrun`        | Error      | RX overrun, framing error, parity error          |
| `tc_reset_mid_tx`   | Corner     | Reset asserted during active transmit            |
| `tc_stress_back2back`| Stress    | Back-to-back frames, no gaps                     |

### 11.3 Coverage Goals

| Coverage Type        | Target |
|----------------------|--------|
| Line / branch / toggle | 100% |
| FSM state coverage   | 100%   |
| FSM transition       | 100%   |
| Functional           | 95%    |
| Register field toggle| 100%   |
| Interrupt cross      | 100%   |

### 11.4 Static Checks

- [ ] Lint (Spyglass / Verilator): zero errors, zero warnings
- [ ] CDC (Synopsys SpyGlass CDC): clean
- [ ] Formal: reset correctness, interrupt generation properties

---

## 12. Known Limitations

| ID   | Description          | Workaround |
|------|----------------------|------------|
| L001 | _None at this time_  | —          |

---

## 13. Revision History

| Version | Date         | Author     | Description       |
|---------|-------------|------------|-------------------|
| 0.1     | <YYYY-MM-DD> | <author>   | Initial draft     |
````

---

### 3. Self-Review Checklist

After generating, verify:

- [ ] Section 4 (Interrupts) is a standalone top-level section — not buried in functional description
- [ ] Section 5 (Low-power modes) exists and covers Sleep/Stop/Standby behavior explicitly
- [ ] Section 6.8 has a register map summary table (all registers in one place)
- [ ] Every register field has an access type: RW, RO, WO, W1C, W1S, or RC
- [ ] Reset values are specified for every register
- [ ] No placeholder text left unflagged where user provided the information
- [ ] Introduction mentions which device variants the IP is in
- [ ] Functional description is subdivided by operating mode (not one giant paragraph)

### 4. Save the File

```bash
mkdir -p ./docs/<ip-name>
# Write via Write tool
```

Confirm the saved path to the user.

### 5. Offer Enhancements

After saving, offer to:
- Add more registers
- Expand a functional description sub-section
- Add timing waveform diagrams (Wavedrom JSON format)
- Generate a C header file from the register map
- Generate a SystemVerilog register package

## Wrap up

Tell the user:
- Where the file was saved (`./docs/<ip-name>/<ip-name>_spec.md`)
- How many sections are fully populated vs have `_TODO_` markers
- Top 2–3 things that would most improve the document
- Suggest next steps: design review, adding waveforms, RTL skeleton generation
