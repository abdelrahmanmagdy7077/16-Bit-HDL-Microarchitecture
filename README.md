# 16 Bit HDL Microarchitecture

![Architecture](https://img.shields.io/badge/Architecture-16--Bit_RISC-blue?style=for-the-badge)
![Language](https://img.shields.io/badge/Language-Structural_HDL-orange?style=for-the-badge)
![Verification](https://img.shields.io/badge/Verification-100%25_Pass-success?style=for-the-badge)
![License](https://img.shields.io/badge/License-MIT-lightgrey?style=for-the-badge)

A full gate level implementation of the 16 bit Hack computer from the book *The Elements of Computing Systems* by Noam Nisan and Shimon Schocken (MIT Press), which is widely known as the Nand2Tetris project. 

This repository contains the Hardware Description Language (HDL) code, circuit wiring, and verification scripts used to build a working CPU, memory hierarchy, and general purpose computer from the ground up, starting from primitive NAND gates and the sequential DFF primitive provided by the HDL.

---

## Project Scope and Implementation

This repository contains the complete hardware implementation of the design. Every component, from the basic logic gates up to the CPU and memory hierarchy, is coded and wired using structural HDL.

The architectural blueprint comes from *The Elements of Computing Systems*. Nisan and Schocken designed the overarching Hack architecture, the instruction set (ISA), and the simulation software used to verify the circuits. This project focuses on the engineering execution, wiring the HDL components together to implement the datapath and control logic according to their published specification.

---

## System Architecture

The hardware is structured across four distinct layers. Each layer relies strictly on the verified chips from the layer below it:

### 1. Logic Gates and Buses
* Built from scratch using primitive NAND gates.
* Handles fundamental logic (`AND`, `OR`, `XOR`, `NOT`), multi bit buses, and data routing (`MUX` and `DMUX` chips).

### 2. Arithmetic Logic Unit (ALU)
* A custom 16 bit processing unit that executes math and logic operations in a single clock cycle.
* Controlled by 6 instruction bits to handle addition, subtraction, negation, and bitwise manipulation.
* Outputs two hardware flags (`zr` for zero, `ng` for negative) to drive CPU conditional branching.

### 3. Memory Hierarchy
* Uses D Flip Flops (DFFs) to step from combinational logic into state retaining sequential memory.
* Scales from a basic 16 bit register up to addressable RAM blocks (`RAM8`, `RAM64`, `RAM16K`).
* Features a dedicated Program Counter (PC) with hardware level increment, load, and reset lines.

### 4. CPU and Top Level Computer
* Brings the ALU, registers, and instruction decoding logic together to execute standard 16 bit machine code.
* Manages the continuous fetch, decode, and execute cycle alongside memory I/O and hardware jumps.
* At the circuit level, this is the complete computer capable of running compiled software binaries.

---

## Toolchain and Structural HDL

This project uses a minimal Hardware Description Language (HDL) tailored for structural, gate level modeling. 

Unlike commercial Verilog or VHDL, where you can write `a + b` and let the synthesizer automatically infer the adder circuit, this environment enforces **pure structural design**. Every bus, multiplexer tree, flip flop feedback loop, and logic gate is wired together from lower level chips. There are no black boxes. If the computer does math or stores a bit, it is because the arithmetic and memory circuits are structurally composed from simple HDL components.
