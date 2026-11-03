# SSX Silicon 1

A fully functional, verified 32-bit CPU running inside Google Sheets.

**Creator:** Samin Sami (SS)  
**Architecture:** SSX-32 (32-Bit RISC/CISC Hybrid)  
**Target:** Google Sheets  
**License:** MIT  

![SSX Silicon 1 User Interface](./SSX%20UI.png)

---

## Overview

SSX Silicon 1 is a 32-bit processor implemented in a spreadsheet environment. It provides 32 general-purpose registers, a 32-bit arithmetic logic unit, status flag tracking, a 16-kilobyte memory space, and a 32x32 color display matrix driven by video RAM.

Unlike earlier 16-bit spreadsheet experiments that relied on external workbook dependencies and basic formula sets, SSX Silicon 1 is self-contained. It operates under two execution models:

1. **Native formula recalculation:** Uses Google Sheets iterative calculation with circular reference formula latches. Each manual clock toggle or spreadsheet edit recalculates the instruction cycle.
2. **Embedded Apps Script engine:** Uses a V8 JavaScript runtime embedded directly in the spreadsheet to provide interactive buttons, custom menus, and execution speeds reaching hundreds of instructions per second.

## Live demonstration

Watch the processor execute instructions, run diagnostic verification suites, and render graphics inside Google Sheets (1.4x speed):

<p align="center">
  <video src="./SSX-Silicon-1-Live-Demonstration-and-Verification_1.4x_fast.mp4" controls width="100%">
    <a href="./SSX-Silicon-1-Live-Demonstration-and-Verification_1.4x_fast.mp4">
      <img src="./SSX%20UI.png" alt="Watch the SSX Silicon 1 Live Demonstration (MP4)" width="100%">
    </a>
  </video>
</p>

> **Video link:** [Watch or download `SSX-Silicon-1-Live-Demonstration-and-Verification_1.4x_fast.mp4`](./SSX-Silicon-1-Live-Demonstration-and-Verification_1.4x_fast.mp4)

## Architecture specifications

- **Word width:** 32 bits (0 to 4,294,967,295 unsigned; -2,147,483,648 to 2,147,483,647 signed).
- **Register file:** 32 general-purpose 32-bit registers (R0 through R31).
  - `R0`: Hardwired zero register (immutable zero).
  - `R1` to `R26`: General computation, return values, and parameters.
  - `R27`: Auxiliary register holding upper 32 bits of multiplication and division remainders.
  - `R28 (FP)`: Frame pointer.
  - `R29 (SP)`: Stack pointer, initialized to address `0x3FFF`.
  - `R30 (LR)`: Link register for subroutine return addresses.
  - `R31 (PC)`: Program counter alias.
- **Status flags:**
  - `ZF`: Zero flag.
  - `CF`: Carry / borrow flag.
  - `SF`: Sign / negative flag (bit 31).
  - `OF`: Two's complement overflow flag.
- **Memory space:**
  - `0x0000` to `0x0FFF`: Program ROM (4,096 32-bit words).
  - `0x1000` to `0x1FFF`: Data RAM (4,096 32-bit words).
  - `0x2000` to `0x20FF`: Memory-mapped I/O (system timers, random number generator, I/O ports).
  - `0x2400` to `0x27FF`: Video RAM driving the 32x32 graphics display.
  - `0x3F00` to `0x3FFF`: Subroutine call stack.
- **Display matrix:** 32x32 color graphics screen supporting a 16-color RGB palette with conditional cell formatting.
- **Instruction set:** 36 orthogonal instructions supporting arithmetic, logic, barrel shifts, rotates, condition testing, jumps, calls, returns, stack manipulation, and graphics plotting.

## Repository contents

```text
SSX-Silicon-1/
├── SSX_Silicon_1_CPU.xlsx      # Master spreadsheet containing all 8 CPU sheets
├── README.md                   # Project documentation and specifications
├── LICENSE                     # MIT open-source license
├── compiler/
│   ├── ssx_isa.py              # ISA definitions, opcodes, and bitfield helpers
│   ├── ssx_assembler.py        # Two-pass assembler generating binary, hex, and CSV
│   ├── ssx_disassembler.py     # Disassembler converting machine code to assembly
│   └── ssx_emulator.py         # Bit-exact cycle-accurate reference simulator
├── programs/
│   ├── 01_hardware_selftest.ssx# Complete CPU diagnostic suite (golden: 0xCAFEBABE)
│   ├── 02_fibonacci_32bit.ssx  # 47-term 32-bit Fibonacci sequence generator
│   ├── 03_prime_sieve.ssx      # Sieve of Eratosthenes finding primes up to 100
│   ├── 04_matrix_multiply.ssx  # 3x3 integer matrix multiplication
│   └── 05_screen_demo_graphics.ssx # 32x32 color geometric graphics renderer
├── scripts/
│   ├── SSX_Silicon_Engine.js   # Google Apps Script interactive automation engine
│   └── make_sheets_cpu.py      # Python script to build the entire XLSX workbook
├── diagrams/
│   ├── ssx-silicon-1-arch.html # Interactive CPU architecture diagram (Archify)
│   ├── ssx-instruction-cycle.html # 5-stage pipeline workflow diagram (Archify)
│   ├── ssx-silicon-1-arch.json # Typed architecture JSON specification
│   └── ssx-instruction-cycle.json # Typed workflow JSON specification
└── docs/
    ├── ARCHITECTURE.md         # Detailed microarchitecture specification
    ├── ISA_MANUAL.md           # Opcode encodings and assembly syntax manual
    ├── NON_TECH_STEP_BY_STEP_GUIDE.md # Simple walkthrough for beginners and evaluators
    ├── GOOGLE_SHEETS_GUIDE.md  # Complete setup and user walkthrough
    └── VERIFICATION_REPORT.md  # Test logs and mathematical proof of correctness
