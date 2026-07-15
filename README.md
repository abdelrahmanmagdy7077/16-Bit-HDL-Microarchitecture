# 16-Bit HDL Microarchitecture

A complete 16-bit computer processor built from scratch using basic logic gates. This repository contains the Hardware Description Language (HDL) code, circuit designs, and test scripts used to build a working CPU, memory hierarchy, and top-level computer architecture from the ground up.

---

## System Architecture

The hardware is built across four layers. Each layer relies strictly on the verified chips from the layer below it:

### 1. Logic Gates & Buses
* Built directly from primitive NAND gates.
* Includes fundamental logic (`AND`, `OR`, `XOR`, `NOT`), multi-bit buses, and data routing chips (multiplexers and demultiplexers).

### 2. Arithmetic Logic Unit (ALU)
* A custom 16-bit processing chip that handles math and logic operations in a single clock cycle.
* Controlled by 6 input bits to perform addition, subtraction, negation, and bitwise operations.
* Outputs two status flags (`zr` for zero, `ng` for negative) used by the CPU to handle conditional branching.

### 3. Memory Hierarchy
* Built using D-Flip-Flops (DFFs) to create sequential, state-retaining memory.
* Scales from a single 16-bit register up to addressable RAM modules (`RAM8`, `RAM64`, `RAM16K`).
* Includes a dedicated Program Counter (PC) with built-in increment, load, and reset functions.

### 4. CPU & Top-Level Computer
* Combines the ALU, registers, and instruction decoding logic to execute standard 16-bit machine code.
* Handles the continuous fetch-decode-execute cycle, memory read/write operations, and jump commands.
* Connects all components into a top-level computer architecture capable of running compiled binary programs.

---

## Toolchain & Structural HDL

This project uses a specialized Hardware Description Language (HDL) designed for structural gate-level modeling. 

Unlike standard Verilog or VHDL where you can use math shortcuts (like `+` or `-`), this HDL requires **pure structural design**. Every bus, logic gate, and clock-edge connection had to be wired by hand using simpler chips. This ensures the entire architecture is truly built and understood from first principles.

---

## Verification and Testing

Every chip in this repository was tested and verified using a Java-based Hardware Simulator. The testing checks:

* **Logic Accuracy:** Using `.tst` scripts to verify that every combinational chip gives the exact expected output for all possible inputs.
* **Timing & Memory:** Ensuring registers, RAM chips, and clock-cycle states hold data correctly without losing information or causing race conditions.
* **CPU Behavior:** Running full instruction cycles to confirm proper branching, data routing, and execution timing against expected `.cmp` outputs.

---

## Specification Reference

The hardware specifications, instruction set architecture (ISA), and HDL syntax used in this project are based on the computer engineering framework from the textbook *The Elements of Computing Systems* by Noam Nisan and Shimon Schocken (MIT Press).



