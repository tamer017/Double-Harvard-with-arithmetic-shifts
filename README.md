# Double Harvard Architecture with Arithmetic Shifts

> **Java simulation of a fictional Double-Harvard 3-stage pipelined processor with 64 GPRs, a 5-flag status register, 12-instruction ISA, and arithmetic shift operations.**

[![Java](https://img.shields.io/badge/Language-Java-orange.svg)](https://www.java.com/)
[![Architecture](https://img.shields.io/badge/Architecture-Harvard_Pipeline-blue.svg)]()

---

## Overview

This project simulates a fictional processor design and architecture using Java.

---

## 1. Memory Architecture

### Architecture: Harvard

Harvard Architecture is the digital computer architecture whose design is based on the concept where there are **separate storage and separate buses** (signal path) for instruction and data. It was developed to overcome the bottleneck of Von Neumann Architecture.

### Instruction Memory — 1024 × 16

![1](https://user-images.githubusercontent.com/83555471/151936981-f49a4e78-a68e-42e0-8675-cf656ad6a8df.PNG)

- Addresses: 0 to 2¹° − 1 (0 to 1023)
- Each memory block contains 1 word = **16 bits** (2 bytes)
- Word addressable
- Stores program instructions

### Data Memory — 2048 × 8

![2](https://user-images.githubusercontent.com/83555471/151936984-476e5b81-57c0-4a13-a6c3-49cc23dc5738.PNG)

- Addresses: 0 to 2¹¹ − 1 (0 to 2047)
- Each memory block contains 1 word = **8 bits** (1 byte)
- Word/byte addressable (1 word = 1 byte)
- Stores data

### Registers: 66

- **64 General-Purpose Registers (R0 – R63)**, each 8 bits
- **1 Status Register (SREG)** — 5 flags:

![3](https://user-images.githubusercontent.com/83555471/151936986-56409704-0d22-439d-83b3-85bd4ca73bf1.PNG)

| Flag | Name | Description |
|---|---|---|
| **C** | Carry | Set when result > Byte.MAX_VALUE |
| **V** | Overflow | Set when signed result overflows |
| **N** | Negative | Set when result is negative |
| **S** | Sign | S = N ⊕ V |
| **Z** | Zero | Set when result = 0 |

- **1 Program Counter (PC)** — 16 bits (not 8 bits), holds address of current instruction

---

## 2. Instruction Set Architecture

### Instruction Size: 16 bits
### Instruction Types: 2

![4](https://user-images.githubusercontent.com/83555471/151936972-fd47fe00-0e1a-493d-80d9-c53bf8d391da.PNG)

### Instruction Count: 12 (opcodes 0 – 11)

![5](https://user-images.githubusercontent.com/83555471/151936975-11fec273-d517-4467-83c9-a70aa8299464.PNG)

### Status Register Flag Updates

| Flag | Updated by |
|---|---|
| **Carry (C)** | ADD, SUB, MUL |
| **Overflow (V)** | ADD, SUB |
| **Negative (N)** | ADD, SUB, MUL, ANDI, EOR, SAL, SAR |
| **Sign (S)** | ADD, SUB |
| **Zero (Z)** | ADD, SUB, MUL, ANDI, EOR, SAL, SAR |

---

## 3. Datapath

### Stages: 3

All instructions must pass through all 3 stages:

- **IF (Instruction Fetch)**: Fetches the next instruction from main memory using the PC address.
- **ID (Instruction Decode)**: Decodes the instruction and reads required operands from the register file.
- **EX (Execute)**: Executes the instruction. Performs all ALU operations, memory access (load/store), and writeback to register file.

### Pipeline: 3 instructions maximum in parallel

**Clock cycles formula**: `3 + ((n − 1) × 1)` where n = number of instructions

Example — 7 instructions: `3 + (6 × 1) = 9 clock cycles`

![6](https://user-images.githubusercontent.com/83555471/151936978-cd53c8b1-8dd9-4097-b15a-f23023d3355f.PNG)

---

## How to Run

1. Write your program in assembly language in a text file.
2. Run the main class.

### Console Output After Each Clock Cycle:

- Clock Cycle number
- Pipeline stages: which instruction is at each stage + input parameters/values
- Updates to registers (if any register value changed)
- Updates to data memory (if any value was stored or updated)
- Full content of all registers after the last clock cycle
- Full content of memory after the last clock cycle

---

## Skills & Concepts

`Computer Architecture` `Harvard Architecture` `Pipeline Simulation` `Java` `Custom ISA` `Arithmetic Shifts` `Register File` `Status Register` `Fetch-Decode-Execute` `Assembly Language`

---

## Author

**Ahmed Tamer Assy** — [GitHub](https://github.com/tamer017) | Machine Learning Researcher @ Volkswagen AG
