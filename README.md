# Double Harvard Architecture Pipeline Simulator

> **Software simulator for a custom 3-stage pipelined processor with Double Harvard Architecture, including a hand-written two-pass Java assembler and data hazard detection.**

[![Java](https://img.shields.io/badge/Language-Java-orange.svg)](https://www.java.com/)
[![Architecture](https://img.shields.io/badge/Architecture-Harvard_Pipeline-blue.svg)]()

---

## Overview

This project implements a full software simulator of a custom **3-stage pipelined processor** based on the Double Harvard Architecture. The simulator faithfully models the CPU pipeline including data hazard detection, branch resolution, and a custom ISA featuring arithmetic shift instructions.

A hand-written **two-pass Java assembler** translates custom assembly programs into binary machine code that can be loaded and executed by the simulator.

---

## Architecture

### Double Harvard Memory Model

Unlike standard Harvard Architecture (separate instruction + data memory), Double Harvard adds a third memory space:

```
┌────────────────┐
│  Instruction    │  ← Fetch stage reads from here
│  Memory (IMEM)  │
└────────────────┘
┌────────────────┐
│  Data Segment   │  ← General data (heap/globals)
│  Memory (DMEM)  │
└────────────────┘
┌────────────────┐
│  I/O Memory     │  ← Memory-mapped I/O
└────────────────┘
```

### 3-Stage Pipeline

```
Clock:    1    2    3    4    5    6
Instr 1:  IF   ID   EX
Instr 2:       IF   ID   EX
Instr 3:            IF   ID   EX
```

| Stage | Name | Operations |
|---|---|---|
| **IF** | Instruction Fetch | PC → IMEM, PC+1 |
| **ID** | Instruction Decode | Register file read, control signals |
| **EX** | Execute | ALU operation, memory access, writeback |

### Data Hazard Detection

The simulator detects **Read-After-Write (RAW)** hazards:
```
Instr N:    ADD R1, R2, R3   (writes R1 in EX)
Instr N+1:  SUB R4, R1, R5   (reads R1 in ID)
```
When detected: inserts a **bubble (stall cycle)** between the instructions, penalizing throughput by 1 cycle per dependency.

### Branch Resolution
Branches resolved in **EX stage** — introduces 2-cycle penalty (pipeline flush on taken branches).

---

## Custom ISA

| Instruction | Format | Description |
|---|---|---|
| `ADD Rd, Rs1, Rs2` | R-type | Integer addition |
| `SUB Rd, Rs1, Rs2` | R-type | Integer subtraction |
| `LDW Rd, offset(Rs)` | I-type | Load word from DMEM |
| `STW Rs, offset(Rd)` | I-type | Store word to DMEM |
| `BEQ Rs1, Rs2, label` | B-type | Branch if equal |
| `SAL Rd, Rs, imm` | R-type | **Shift Arithmetic Left** |
| `SAR Rd, Rs, imm` | R-type | **Shift Arithmetic Right** (sign-extending) |

**SAL/SAR** are the key additions over a standard RISC ISA — SAR preserves the sign bit, unlike logical shifts.

---

## Two-Pass Assembler

**Pass 1:** Scan all labels, build symbol table with their addresses.

**Pass 2:** Translate mnemonics to binary, resolve label references using the symbol table.

```
Assembly Source (.asm)
        │
  Pass 1: Build Symbol Table
  {loop: 0x004, end: 0x012, ...}
        │
  Pass 2: Encode Instructions
  ADD R1, R2, R3 → 0b000100010001100
        │
  Binary Output (.bin) → loaded into IMEM
```

---

## Installation

```bash
git clone https://github.com/tamer017/Double-Harvard-with-arithmetic-shifts.git
cd Double-Harvard-with-arithmetic-shifts
javac -d bin src/**/*.java
java -cp bin Main programs/test_program.asm
```

---

## Skills & Concepts

`Computer Architecture` `Pipeline Simulation` `Hazard Detection` `Two-Pass Assembler` `Java` `Custom ISA` `Harvard Architecture` `Arithmetic Shifts` `Branch Resolution` `Memory Models`

---

## Author

**Ahmed Tamer Assy** — [GitHub](https://github.com/tamer017) | Machine Learning Researcher @ Volkswagen AG
