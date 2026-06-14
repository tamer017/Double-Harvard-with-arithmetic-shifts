# Double Harvard Architecture with Arithmetic Shifts — Pipeline Simulator

> A **3-stage pipelined processor simulator** implementing the Double Harvard Architecture with arithmetic shift operations, built in Java.

[![Language](https://img.shields.io/badge/Language-Java-red?style=flat-square)](https://www.java.com/)
[![Architecture](https://img.shields.io/badge/Architecture-Double%20Harvard-blue?style=flat-square)]()
[![Type](https://img.shields.io/badge/Type-CPU%20Simulator-orange?style=flat-square)]()

---

## Overview

This project implements a **software simulation of a custom 3-stage pipelined processor** based on the Double Harvard Architecture. The simulator faithfully models the fetch–decode–execute pipeline, resolves data and control hazards, and includes a **custom assembler** that translates a human-readable instruction set into machine code executable by the simulator.

The Double Harvard Architecture uses **four independent memory spaces** (instruction memory, data memory, I/O space, and an additional data bus), enabling simultaneous instruction fetch and data access without bus contention — a key advantage over the Von Neumann architecture.

---

## Architecture

### Pipeline Stages

```
[Instruction Memory]
        |
        v
  ┌───────────────┐
  │  FETCH (IF)    │   ←  PC register, instruction memory bus
  └───────────────┘
        |
        v
  ┌───────────────┐
  │  DECODE (ID)   │   ←  Register file read, immediate sign-extension
  └───────────────┘
        |
        v
  ┌───────────────┐
  │  EXECUTE (EX)  │   ←  ALU ops, memory access, writeback, branch resolve
  └───────────────┘
```

### Memory Architecture (Double Harvard)

| Space | Purpose |
|---|---|
| Instruction Memory | Read-only program storage |
| Data Memory | Read/write data bus (separate from instructions) |
| Register File | 8 general-purpose registers (R0–R7) |
| I/O Space | Memory-mapped I/O ports |

---

## Instruction Set

The processor supports a custom **RISC-style ISA** including:

| Category | Instructions |
|---|---|
| Arithmetic | `ADD`, `SUB`, `MUL`, `INC`, `DEC` |
| Logical | `AND`, `OR`, `XOR`, `NOT` |
| Shift | `SHL`, `SHR`, `SAL`, `SAR` (arithmetic shifts with sign extension) |
| Data Transfer | `MOV`, `LOAD`, `STORE` |
| Control Flow | `JMP`, `JZ`, `JN`, `JC`, `CALL`, `RET` |

### Arithmetic Shifts
The `SAL` and `SAR` (Shift Arithmetic Left/Right) instructions preserve the **sign bit** during right shifts, enabling signed integer division by powers of 2 without overflow. This is the key differentiator from logical shifts (`SHL`/`SHR`).

---

## Custom Assembler

The project includes a hand-written Java assembler that:
1. **Tokenizes** assembly source files into lexical units
2. **Resolves labels** via a two-pass algorithm (first pass: symbol table, second pass: address substitution)
3. **Encodes instructions** to binary machine code following the custom ISA encoding scheme
4. **Outputs** a `.mem` file loadable directly into the simulator’s instruction memory

---

## Hazard Handling

| Hazard Type | Resolution Strategy |
|---|---|
| Data hazard (RAW) | Pipeline stalls (bubble insertion) |
| Control hazard | Branch resolution in EX stage, flush IF/ID on taken branch |
| Structural hazard | Avoided by architecture (separate instruction/data buses) |

---

## Getting Started

```bash
# Clone the repository
git clone https://github.com/tamer017/Double-Harvard-with-arithmetic-shifts.git
cd Double-Harvard-with-arithmetic-shifts

# Compile the Java simulator
javac -d out src/**/*.java

# Assemble a program
java -cp out Assembler programs/fibonacci.asm output.mem

# Run the simulator
java -cp out Simulator output.mem
```

---

## Skills Demonstrated

- **Computer Architecture:** Pipeline design, hazard detection, Harvard architecture, ISA design
- **Java:** Object-oriented simulation, two-pass assembler, binary encoding
- **Systems Programming:** Low-level register file simulation, memory-mapped I/O modeling
- **Compiler Theory:** Lexical analysis, symbol table construction, two-pass assembly
