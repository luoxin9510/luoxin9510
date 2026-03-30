---
name: asic-ip-spec
description: >
  Generate a complete, professional ASIC IP specification document in Markdown.
  Use when the user wants to write, define, or generate an IP spec, hardware block spec,
  RTL spec, or IP documentation for an ASIC or SoC design.
  Triggers on: "write an IP spec", "generate IP spec", "ASIC spec", "hardware spec",
  "RTL spec", "SoC IP documentation", "/asic-ip-spec".
---

# ASIC IP Spec Generator

Guide the user through gathering all required information for a hardware IP block, then
produce a complete, well-structured IP specification document in Markdown following
industry-standard hardware documentation practices.

## Workflow

Make a todo list and work through each step one at a time.

### 1. Gather IP Overview Information

Use AskUserQuestion to collect the following if not already provided. Ask in groups — don't fire one question at a time for all of these; batch related questions together.

**Batch 1 — Identity & Purpose:**
- IP block name (e.g. `uart_ctrl`, `dma_engine`, `axi_bridge`)
- One-line functional description
- Target process node / technology (e.g. TSMC 7nm, GF 22nm, generic)
- Intended SoC or chip name (optional)
- Document version and author

**Batch 2 — Architecture:**
- Key features (bullet list from user)
- Block diagram description or ASCII art (ask user to describe or paste)
- Top-level interfaces (bus protocols: AXI, APB, AHB, custom; data interfaces; interrupts)
- Number of clock domains
- Number of reset domains

**Batch 3 — Details:**
- Register count / address space size
- Power domains / voltage rails
- Any known timing constraints or frequency targets
- Verification intent (UVM, formal, lint, CDC)

If the user says "just generate it" or provides a high-level description only, make reasonable
industry-standard assumptions and clearly mark them as `> **Assumption:**` in the document.

### 2. Generate the Spec Document

Write the complete Markdown spec using the template below. Fill every section with real
content based on user input. For sections where details weren't provided, add a clearly
marked placeholder: `> _TODO: [what is needed here]_`

Use the Write tool to create the file at:
`./docs/<ip-name>/<ip-name>_spec.md`
(create the directory if it doesn't exist)

---

## Spec Document Template

```markdown
# <IP_NAME> IP Specification

| Field        | Value                        |
|--------------|------------------------------|
| Document     | <IP_NAME> IP Specification   |
| Version      | 0.1                          |
| Status       | Draft                        |
| Author       | <author>                     |
| Date         | <YYYY-MM-DD>                 |
| Target       | <process node / SoC>         |

---

## Table of Contents

1. [Overview](#1-overview)
2. [Features](#2-features)
3. [Block Diagram](#3-block-diagram)
4. [Interface Description](#4-interface-description)
5. [Register Map](#5-register-map)
6. [Functional Description](#6-functional-description)
7. [Clocking](#7-clocking)
8. [Reset](#8-reset)
9. [Timing Requirements](#9-timing-requirements)
10. [Power Domains](#10-power-domains)
11. [Interrupts and Events](#11-interrupts-and-events)
12. [Verification Plan](#12-verification-plan)
13. [Known Limitations](#13-known-limitations)
14. [Revision History](#14-revision-history)

---

## 1. Overview

<2–4 sentence description of what this IP does, why it exists, and where it fits in the SoC.>

---

## 2. Features

- Feature 1
- Feature 2
- Feature 3

---

## 3. Block Diagram

```
                    ┌─────────────────────────────┐
                    │         <IP_NAME>            │
  APB Bus  ─────►  │  ┌─────────────┐             │
                    │  │  Reg File   │             │
  CLK      ─────►  │  └──────┬──────┘             │
  RSTn     ─────►  │         │                    │  ──►  <output>
                    │  ┌──────▼──────┐             │
                    │  │  Datapath   │             │
                    │  └─────────────┘             │
                    └─────────────────────────────┘
```

> _TODO: Replace with actual block diagram or more detailed ASCII art._

---

## 4. Interface Description

### 4.1 Port List

| Port Name     | Direction | Width | Clock Domain | Description                        |
|---------------|-----------|-------|--------------|------------------------------------|
| `clk`         | input     | 1     | —            | Primary clock                      |
| `rst_n`       | input     | 1     | —            | Active-low synchronous reset       |
| `apb_psel`    | input     | 1     | clk          | APB select                         |
| `apb_penable` | input     | 1     | clk          | APB enable                         |
| `apb_pwrite`  | input     | 1     | clk          | APB write strobe                   |
| `apb_paddr`   | input     | 12    | clk          | APB address                        |
| `apb_pwdata`  | input     | 32    | clk          | APB write data                     |
| `apb_prdata`  | output    | 32    | clk          | APB read data                      |
| `apb_pready`  | output    | 1     | clk          | APB ready                          |
| `apb_pslverr` | output    | 1     | clk          | APB error response                 |
| `irq`         | output    | 1     | clk          | Interrupt request (level, active-high) |

> _TODO: Add all IP-specific ports._

### 4.2 Interface Protocols

| Interface | Protocol | Version | Notes              |
|-----------|---------|---------|--------------------|
| Register  | APB3    | v2.0    | 32-bit data width  |

---

## 5. Register Map

### 5.1 Address Map Summary

Base address: `0x0000_0000` (relative to IP base)

| Offset | Register Name | Access | Reset Value  | Description               |
|--------|---------------|--------|--------------|---------------------------|
| `0x00` | `CTRL`        | RW     | `0x0000_0000`| Control register          |
| `0x04` | `STATUS`      | RO     | `0x0000_0000`| Status register           |
| `0x08` | `INT_EN`      | RW     | `0x0000_0000`| Interrupt enable          |
| `0x0C` | `INT_STAT`    | RW1C   | `0x0000_0000`| Interrupt status (W1C)    |

### 5.2 Register Descriptions

#### CTRL — Control Register (Offset `0x00`, RW, Reset: `0x0000_0000`)

| Bits  | Field Name  | Access | Reset | Description                          |
|-------|-------------|--------|-------|--------------------------------------|
| 31:8  | RESERVED    | —      | 0     | Reserved, write 0                    |
| 7:4   | MODE        | RW     | 0x0   | Operating mode select                |
| 3     | FLUSH       | WO     | 0     | Write 1 to flush (self-clearing)     |
| 2     | LOOPBACK    | RW     | 0     | 1 = loopback enable                  |
| 1     | IRQ_EN      | RW     | 0     | 1 = global interrupt enable          |
| 0     | ENABLE      | RW     | 0     | 1 = IP enable                        |

#### STATUS — Status Register (Offset `0x04`, RO, Reset: `0x0000_0000`)

| Bits  | Field Name  | Access | Reset | Description                          |
|-------|-------------|--------|-------|--------------------------------------|
| 31:2  | RESERVED    | —      | 0     | Reserved                             |
| 1     | BUSY        | RO     | 0     | 1 = IP is busy                       |
| 0     | READY       | RO     | 0     | 1 = IP ready for operation           |

> _TODO: Add remaining registers._

---

## 6. Functional Description

### 6.1 Operating Modes

<Describe each operating mode. What the IP does in each mode, how to configure it.>

### 6.2 Data Flow

<Describe the data path from input to output. Include pipeline stages if applicable.>

### 6.3 Control Flow / State Machine

<Describe the main FSM or control flow. If there is a state machine, describe all states
and transitions. An ASCII state diagram is encouraged.>

```
IDLE ──[ENABLE=1]──► ACTIVE ──[done]──► IDLE
                      │
                      └──[error]──► ERROR ──[FLUSH]──► IDLE
```

### 6.4 Interrupt Generation

| Interrupt Source    | Trigger Condition        | Enable Bit    | Status Bit    |
|---------------------|--------------------------|---------------|---------------|
| Operation Complete  | Data transfer done       | INT_EN[0]     | INT_STAT[0]   |
| Error               | Protocol/parity error    | INT_EN[1]     | INT_STAT[1]   |

---

## 7. Clocking

| Clock Name | Frequency      | Source        | Description                        |
|------------|---------------|---------------|------------------------------------|
| `clk`      | up to 500 MHz  | External PLL  | Primary functional clock           |

### 7.1 Clock Domain Crossings (CDC)

| From Domain | To Domain | Signal(s)        | Synchronizer Type  |
|-------------|-----------|------------------|--------------------|
| clk         | clk2      | `data_valid`     | 2FF synchronizer   |

> _TODO: List all CDCs or state "single clock domain — no CDCs"._

---

## 8. Reset

| Reset Signal | Polarity    | Type         | Scope               |
|--------------|-------------|--------------|---------------------|
| `rst_n`      | Active low  | Synchronous  | Full IP             |

### 8.1 Reset Behavior

- All registers return to their reset values listed in Section 5.
- All FIFOs are flushed.
- All output signals are driven to their inactive state.
- Reset must be asserted for a minimum of **2 clock cycles**.

---

## 9. Timing Requirements

| Parameter           | Min  | Typ  | Max  | Unit | Condition            |
|--------------------|------|------|------|------|----------------------|
| Clock period        | 2.0  | —    | —    | ns   | 500 MHz max          |
| Input setup time    | 0.2  | —    | —    | ns   | Relative to clk rise |
| Input hold time     | 0.1  | —    | —    | ns   | Relative to clk rise |
| Output valid delay  | —    | —    | 1.5  | ns   | Relative to clk rise |

> _TODO: Populate with actual STA/timing constraints._

---

## 10. Power Domains

| Domain Name | Voltage    | Always-On | Retention | Description           |
|-------------|-----------|-----------|-----------|----------------------|
| `VDD_CORE`  | 0.8V       | Yes       | No        | Primary logic domain |

### 10.1 Power Consumption Estimates

| Mode      | Dynamic Power | Leakage  | Notes                  |
|-----------|--------------|----------|------------------------|
| Active    | TBD mW       | TBD μW   | At nominal freq/voltage|
| Idle      | TBD mW       | TBD μW   | ENABLE=0               |

---

## 11. Interrupts and Events

| IRQ # | Name             | Type        | Description                      |
|-------|------------------|-------------|----------------------------------|
| 0     | OP_COMPLETE      | Level       | Operation completed successfully |
| 1     | ERROR            | Level       | Error detected                   |

All interrupts are cleared by writing 1 to the corresponding bit in `INT_STAT` (W1C).

---

## 12. Verification Plan

### 12.1 Testbench Architecture

| Component         | Type        | Description                              |
|-------------------|-------------|------------------------------------------|
| APB VIP           | UVM Agent   | Drives APB transactions                  |
| Reference Model   | SystemVerilog | Golden model for output checking       |
| Scoreboard        | UVM         | Compares DUT output vs reference         |
| Coverage Collector| UVM         | Functional coverage collection           |

### 12.2 Test Cases

| Test Name             | Category    | Description                                |
|-----------------------|-------------|---------------------------------------------|
| `tc_reset`            | Basic       | Reset behavior verification                |
| `tc_reg_access`       | Register    | Read/write all registers, reset values     |
| `tc_basic_op`         | Functional  | Basic operation in default mode            |
| `tc_all_modes`        | Functional  | Exercise all operating modes               |
| `tc_interrupt`        | Interrupt   | All interrupt sources, enable/disable      |
| `tc_error_injection`  | Error       | Protocol errors, illegal accesses         |
| `tc_backpressure`     | Stress      | Backpressure and flow control              |
| `tc_reset_mid_op`     | Corner      | Reset asserted during active operation     |

### 12.3 Coverage Goals

| Coverage Type        | Target  |
|----------------------|---------|
| Code coverage        | 100%    |
| FSM state coverage   | 100%    |
| FSM transition coverage | 100% |
| Functional coverage  | 95%     |
| Register field toggle| 100%    |

### 12.4 Formal and Lint

- [ ] Lint: Spyglass or equivalent — zero errors, zero warnings
- [ ] CDC: Synopsys SpyGlass CDC or Mentor 0-In — clean
- [ ] Formal: Property checking on reset behavior and key protocols

---

## 13. Known Limitations

| ID   | Description                                     | Workaround                  |
|------|-------------------------------------------------|-----------------------------|
| L001 | _None at time of writing_                       | —                           |

---

## 14. Revision History

| Version | Date       | Author     | Description          |
|---------|------------|------------|----------------------|
| 0.1     | <YYYY-MM-DD> | <author> | Initial draft        |
```

---

### 3. Review and Refine

After generating the document, do a self-review pass:

- [ ] Every section has real content or a clear `_TODO_` marker
- [ ] All register fields have access types (RW, RO, WO, W1C, W1S)
- [ ] Port widths and directions are consistent throughout
- [ ] CDC section is complete (or explicitly states single clock domain)
- [ ] Reset behavior is fully specified
- [ ] No placeholder text left unflagged

If the user provided enough detail to fill a section fully, fill it. Don't leave `_TODO_`
where the user already gave you the information.

### 4. Save the File

Write the document to:
```
./docs/<ip-name>/<ip-name>_spec.md
```

If `./docs/<ip-name>/` doesn't exist, create it. Confirm the file path to the user.

### 5. Offer to Expand Any Section

After saving, ask if the user wants to:
- Add more registers
- Expand the functional description
- Add waveform diagrams (timing diagrams in ASCII/Wavedrom)
- Generate a register header file (C/SystemVerilog) from the register map

## Wrap up

Tell the user:
- Where the spec was saved
- How many sections were fully populated vs marked TODO
- What information would most improve the document
- Suggest next steps: peer review, adding waveforms, generating RTL skeleton
