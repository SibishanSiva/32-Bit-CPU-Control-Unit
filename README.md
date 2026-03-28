# 32-Bit CPU Control Unit in VHDL

This project is a VHDL-based 32-Bit CPU Control Unit designed and verified using Quartus II on an Altera FPGA. This was originally a lab from my class, COE608 (Computer Organization and Architecture), where I needed to design a control unit that drives the full data-path of a 32-bit CPU — decoding instructions, managing a 3-state FSM, and generating all control signals for the registers, ALU, MUXes, and memory unit — and verify its behaviour through functional simulation waveforms.

## Features
* 3-state Moore FSM (T0, T1, T2) that sequences instruction execution across three clock cycles, with an enable/reset input that holds the CPU at state T0 until released.
* Operation Decoder that reads the opcode (INST[31..28]) and function code (INST[27..24]) alongside Carry and Zero status bits to correctly drive all data-path control signals.
* Memory Signal Generator that handles Write Enable (wen) and Enable (en) signals with correct setup and hold timing for load and store memory operations, sensitive to both clk and mclk.
* Supports 25+ instructions: LDAI, LDBI, LDA, LDB, STA, STB, LUI, JMP, BEQ, BNE, ADD, ADDI, SUB, INCA, DECA, ROL, ROR, AND, ANDI, ORI, CLRA, CLRB, CLRC, CLRZ, TSTZ, and TSTC.
* Conditional branching via TSTZ and TSTC, which conditionally increment the PC only when the Z or C status flag is asserted.
* Complete control signal table covering CLR/LD for registers A, B, C, Z, PC load/increment, ALU opcode, EN/WEN, A/B MUX, REG MUX, DATA MUX, IM_MUX1, and IM_MUX2 for every supported instruction.
* Full functional simulation waveforms validating correct control signal output for all 25+ instructions across all three execution states.

## | Waveform Simulations | ![Link](waveform.pdf) |
