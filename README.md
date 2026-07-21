
# 16-Bit Hack Computer System & Assembler Toolchain

![Architecture](https://img.shields.io/badge/Architecture-16--Bit_RISC-blue?style=for-the-badge)
![Hardware](https://img.shields.io/badge/Hardware-Structural_HDL-orange?style=for-the-badge)
![Toolchain](https://img.shields.io/badge/Assembler-Python%20%2F%20CLI-yellow?style=for-the-badge)
![Verification](https://img.shields.io/badge/Verification-100%25_Pass-success?style=for-the-badge)

A complete, bottom-up implementation of the 16-bit Hack computer from the book *The Elements of Computing Systems* (Nand2Tetris Part 1) by Noam Nisan and Shimon Schocken. 

This repository starts at the absolute bottom of computing: **primitive NAND gates**. From there, it builds up the foundational logic, an arithmetic processing unit, a sequential memory hierarchy, and the central processing unit (CPU). Finally, it crosses the bridge from hardware into software by implementing a custom Assembler compiler that translates symbolic assembly language into raw 16-bit binary executable code.

There are no black boxes here. If this computer adds two numbers, stores a bit in RAM, or renders a pixel on a screen, it is because the physical circuits and compiler tools were engineered from scratch to make it happen.

> **Note on the Software Hierarchy:** This repository focuses strictly on hardware architecture, datapath wiring, and bare-metal assembly (Projects 1–6). The high-level software stack—including the Virtual Machine bytecode translator, the Jack compiler, and the Operating System kernel (Projects 7–12)—is built in a companion repository: [Jack-to-Hack Software Stack](https://github.com/yourusername/jack-to-hack-software-stack).

---

## Repository Structure & Progression

The project directory is numbered sequentially to mirror the engineering roadmap of the computer's construction:

```text
├── 01_boolean_logic/         # Primitive logic gates and data routing chips
├── 02_boolean_arithmetic/    # Adders, incrementers, and the Arithmetic Logic Unit (ALU)
├── 03_sequential_logic/      # Flip-flops, registers, RAM hierarchy, and Program Counter
├── 04_machine_language/      # Hack assembly programs demonstrating I/O and arithmetic
├── 05_computer_architecture/ # The central processing unit (CPU) and top-level computer integration
└── 06_assembler/             # Custom cross-compiler translating .asm files to binary machine code

```

---

## The Hardware Stack (Projects 1, 2, 3, and 5)

The hardware architecture is divided into four strict layers of abstraction. Each layer relies entirely on the verified chips from the layer beneath it:

| Layer | Module | Key Deliverables | Engineering Concept |
| --- | --- | --- | --- |
| **1** | **Boolean Logic** | `NOT`, `AND`, `OR`, `XOR`, `MUX`, `DMUX` + 16-bit bus variants | **Combinational Logic:** Building basic data routing and decision-making trees using only primitive NAND gates. |
| **2** | **Arithmetic** | `HalfAdder`, `FullAdder`, `Add16`, `ALU` | **Integer Arithmetic:** Implementing two's-complement math and a custom ALU controlled by 6 instruction bits to compute functions and output branching flags (`zr`, `ng`). |
| **3** | **Memory** | `Bit`, `Register`, `RAM8` through `RAM16K`, `PC` | **Sequential Logic:** Using clock-driven D-Flip-Flops (DFFs) to break out of pure combinational logic, creating state-retaining memory and a hardware Program Counter. |
| **4** | **Architecture** | `Memory`, `CPU`, `Computer` | **System Integration:** Wiring the ALU, registers, and memory-mapped I/O (Screen/Keyboard) together into a CPU that executes a continuous Fetch-Decode-Execute cycle. |

### The Power of Structural HDL

The circuits in this project are written in a specialized Hardware Description Language (HDL) designed for **pure structural modeling**. Unlike commercial Verilog or VHDL where you can write high-level behavioral code (like `a + b`) and let a synthesizer infer the circuit, this toolchain enforces explicit wiring. Every single bus, multiplexer tree, and feedback loop is manually connected pin-to-pin.

---

## The Software Bridge (Projects 4 and 6)

A hardware CPU is useless without instructions to run. The second half of this repository bridges the gap between physical silicon and executable software:

### 1. Hack Assembly Language (Chapter 4)

Before writing a compiler, we must understand the Instruction Set Architecture (ISA). This folder contains low-level programs written in Hack Assembly (`.asm`) to test the CPU's capabilities:

* **Arithmetic Execution:** Programs like `Mult.asm` that implement multiplication through software loops, as the hardware ALU only supports addition and subtraction.
* **Memory-Mapped I/O:** Programs like `Fill.asm` that listen to keyboard memory registers and dynamically manipulate screen memory addresses to draw or clear pixels in real time.

### 2. The Assembler Toolchain (Chapter 6)

To run assembly programs on the hardware CPU, symbolic text must be compiled into 16-bit machine binary (`.hack`). This directory contains a custom assembler written from scratch to automate that translation.

#### How the Assembler Works:

* **Lexical Parsing:** Strips away whitespace and comments, unpacking raw lines of code into underlying instruction fields (A-Instructions for memory loading, C-Instructions for computation and branching).
* **Symbol Table Resolution:** Operates as a **two-pass compiler**. The first pass scans the code to record user-defined loop labels (e.g., `(LOOP)`). The second pass translates variables into physical memory addresses starting at RAM address `16`.
* **Binary Emission:** Maps parsed instruction mnemonics (like `D=D+M` or `JMP`) into their exact 16-bit binary equivalents, generating the final machine code ready to be loaded into the CPU's instruction memory.

---

## Verification & Testing

Every single component in this repository is rigorously tested against formal hardware specifications:

* **Hardware Verification:** All `.hdl` chips are tested using the Nand2Tetris Hardware Simulator. They are evaluated against automated test scripts (`.tst`) and must achieve a 100% match against expected output compare files (`.cmp`).
* **Software Verification:** The custom assembler is verified by compiling `.asm` source files and comparing the resulting binary output line-by-line against the official compiler's binary output using the provided CPU Emulator.
