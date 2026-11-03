# SSX Silicon 1

A fully functional, verified 32-bit CPU running inside Google Sheets.

**Creator:** Samin Sami (SS)  
**Architecture:** SSX-32 (32-Bit RISC/CISC Hybrid)  
**Target:** Google Sheets  
**License:** [MIT](./LICENSE)  

[**Research Paper (PDF)**](./SSX_Silicon_1_Research_Paper.pdf) · [**Architecture Spec (PDF)**](./SSX_Silicon_1_Architecture_Specification.pdf) · [**ISA Manual (PDF)**](./SSX_Silicon_1_ISA_Reference_Manual.pdf) · [**Verification Report (PDF)**](./SSX_Silicon_1_Verification_Report.pdf) · [**Beginner Guide (PDF)**](./SSX_Silicon_1_Beginner_Guidebook.pdf) · [**Download Full Release (.zip)**](./SSX-Silicon-1-Release.zip)

---

![SSX Silicon 1 User Interface](./SSX%20UI.png)

## Overview

SSX Silicon 1 is a 32-bit processor implemented in a spreadsheet environment. It provides 32 general-purpose registers, a 32-bit arithmetic logic unit, status flag tracking, a 16-kilobyte memory space, and a 32x32 color display matrix driven by video RAM.

Unlike earlier 16-bit spreadsheet experiments that relied on external workbook dependencies and basic formula sets, SSX Silicon 1 is self-contained. It operates under two execution models:

1. **Native formula recalculation:** Uses Google Sheets iterative calculation with circular reference formula latches. Each manual clock toggle or spreadsheet edit recalculates the instruction cycle.
2. **Embedded Apps Script engine:** Uses a V8 JavaScript runtime embedded directly in the spreadsheet to provide interactive buttons, custom menus, and execution speeds reaching hundreds of instructions per second.

---

## Core microarchitecture

![SSX Silicon 1 32-Bit Microarchitecture](./ssx-silicon-1---32-bit-microarchitecture%20%281%29.jpeg)

The SSX-32 core architecture integrates a dedicated 32-bit arithmetic logic unit (ALU), an orthogonal 32-register bank, a unified 16KB memory fabric mapped across ROM, RAM, I/O ports, and VRAM, alongside hardware status flag tracking (`ZF`, `CF`, `SF`, `OF`).

---

## Instruction execution pipeline

![SSX Silicon 1 Instruction Execution Pipeline](./ssx-silicon-1---instruction-execution-pipeline%20%281%29.jpeg)

Execution flows through a deterministic multi-stage cycle, cleanly separating instruction fetch, decode, ALU execution, memory access, and register writeback phases.

---

## Live demonstration

Watch the processor execute instructions, run diagnostic verification suites, and render graphics inside Google Sheets (1.4x speed):

<!-- DRAG AND DROP SSX-Silicon-1-Live-Demonstration-and-Verification_1.4x_fast.mp4 FROM YOUR COMPUTER ONTO THE LINE BELOW IN GITHUB'S WEB EDITOR TO ENABLE THE NATIVE INLINE VIDEO PLAYER -->

> **Video Demonstration:** [Open or download `SSX-Silicon-1-Live-Demonstration-and-Verification_1.4x_fast.mp4`](./SSX-Silicon-1-Live-Demonstration-and-Verification_1.4x_fast.mp4)

---

## Hardware verification and test results

Every instruction and sample program was verified against our bit-exact reference emulator:

- **Test 1 (Hardware self-test):** Exercises all 32 ALU operations, signed overflow, stack calls, and memory access. Passes in 77 cycles with golden signature `0xCAFEBABE` in register `R1`.
- **Test 2 (32-bit Fibonacci):** Generates 47 Fibonacci terms in RAM without truncation. Produces `F(46) = 1,836,311,903` in register `R1`.
- **Test 3 (Sieve of Eratosthenes):** Evaluates array indexing and branch loops to find all primes up to 100. Correctly identifies 25 primes.
- **Test 4 (3x3 Matrix multiplication):** Multiplies two 3x3 integer matrices in memory. Produces verified matrix `[30, 24, 18, 84, 69, 54, 138, 114, 90]`.
- **Test 5 (32x32 Color graphics engine):** Plots borders, crosshairs, and a central emblem to VRAM at `0x2400`. Illuminates 232 colored pixels on the display screen.

![SSX Silicon 1 Hardware Test Verification](./SSX%20Test%20Verification.png)

---

## Technical documentation and papers

All PDF manuals and papers below can be previewed directly inside GitHub's built-in document viewer:

| Document | Description | Direct Preview |
| :--- | :--- | :--- |
| **Research Paper** | Complete technical paper detailing the SSX-32 architecture, formula latches, and verification methodology | [Preview PDF](./SSX_Silicon_1_Research_Paper.pdf) |
| **Architecture Specification** | Microarchitecture design, datapath layout, register file, memory map, and display matrix | [Preview PDF](./SSX_Silicon_1_Architecture_Specification.pdf) |
| **ISA Reference Manual** | 32-bit instruction encodings, opcode tables, addressing modes, and assembly syntax | [Preview PDF](./SSX_Silicon_1_ISA_Reference_Manual.pdf) |
| **Verification Report** | Cycle-accurate test logs, emulator comparisons, and mathematical proofs of correctness | [Preview PDF](./SSX_Silicon_1_Verification_Report.pdf) |
| **Beginner Guidebook** | Non-technical step-by-step walkthrough for opening, running, and inspecting the processor | [Preview PDF](./SSX_Silicon_1_Beginner_Guidebook.pdf) |
| **Complete Release Package** | Full archive containing `.xlsx` CPU workbook, Python compiler suite, sample `.ssx` programs, and scripts | [Download `.zip`](./SSX-Silicon-1-Release.zip) |

---

## Architecture specifications

- **Word width:** 32 bits (`0` to `4,294,967,295` unsigned; `-2,147,483,648` to `2,147,483,647` signed).
- **Register file:** 32 general-purpose 32-bit registers (`R0` through `R31`).
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

---

## Release package contents (`SSX-Silicon-1-Release.zip`)

```text
SSX-Silicon-1/
├── SSX_Silicon_1_CPU.xlsx      # Master spreadsheet containing all 8 CPU sheets
├── README.md                   # Project documentation and specifications
├── LICENSE                     # MIT open-source license
├── compiler/
│   ├── ssx_isa.py              # ISA definitions, opcodes, and bitfield helpers
│   ├── ssx_assembler.py        # Two-pass assembler generating binary, hex, and CSV
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
```

---

## Quick start guide

### Opening in Google Sheets

1. Download and extract [`SSX-Silicon-1-Release.zip`](./SSX-Silicon-1-Release.zip).
2. Open [Google Sheets](https://sheets.google.com).
3. Go to `File > Import > Upload`, and upload `SSX_Silicon_1_CPU.xlsx`.
4. Choose `Replace spreadsheet` and confirm.
5. Go to `File > Settings > Calculation`.
6. Turn on `Iterative calculation`, set `Max number of iterations` to `1`, and click `Save settings`.

### Enabling interactive controls and menus

1. In Google Sheets, click `Extensions > Apps Script`.
2. Paste the contents of `scripts/SSX_Silicon_Engine.js` into the script editor.
3. Save the project and refresh the spreadsheet tab.
4. Use the new `SSX Silicon 1` top menu to run programs, step cycles, load sample demos, and benchmark speed.

---

## Assembling custom code

Use the Python assembler inside the release package to compile any `.ssx` source file:

```bash
cd compiler
python ssx_assembler.py ../programs/01_hardware_selftest.ssx selftest
```

The assembler generates:

- `selftest.bin`: Raw binary image.
- `selftest.hex`: Address-annotated hexadecimal listing.
- `selftest.lst`: Detailed assembly listing.
- `selftest_sheets.csv`: CSV table formatted for direct pasting into the `ROM_STORAGE` sheet.

---

## Interactive architecture diagrams

The repository includes standalone interactive HTML and JSON diagrams built with Archify:

- [CPU Microarchitecture Diagram (`ssx-silicon-1-arch.html`)](./ssx-silicon-1-arch.html) ([JSON Spec](./ssx-silicon-1-arch.json)): Explorable component diagram showing the 32-bit core, register bank, memory fabric, and user interface.
- [Instruction Pipeline Workflow (`ssx-instruction-cycle.html`)](./ssx-instruction-cycle.html) ([JSON Spec](./ssx-instruction-cycle.json)): 5-stage instruction cycle tracing fetch, decode, ALU execution, memory access, and register writeback.
