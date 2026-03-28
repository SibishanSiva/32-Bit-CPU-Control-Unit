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

## Instruction Control Signal Table

| INST | CLR_IR / LD_IR | LD_PC / INC_PC | CLR_A / LD_A | CLR_B / LD_B | CLR_C / LD_C | CLR_Z / LD_Z | ALU OP | EN / WEN | A/B MUX | REG MUX | Data MUX | IM_MUX1 / IM_MUX2 |
|------|----------------|----------------|--------------|--------------|--------------|--------------|--------|----------|---------|---------|----------|-------------------|
| LDA  | 0/1 | 0/0 | 0/1 | 0/0 | 0/0 | 0/0 | XXX | 1/1 | 1/0 | 1 | 00 | x/xx |
| LDB  | 0/1 | 0/0 | 0/0 | 0/1 | 0/0 | 0/0 | XXX | 1/1 | 0/1 | 1 | 00 | x/xx |
| STA  | 0/1 | 0/0 | 0/1 | 0/0 | 0/0 | 0/0 | XXX | 1/1 | 1/0 | 0 | 00 | x/xx |
| STB  | 0/1 | 0/0 | 0/0 | 0/1 | 0/0 | 0/0 | XXX | 1/1 | 0/1 | 1 | 00 | x/xx |
| JMP  | 0/1 | 1/0 | xx | xx | xx | xx | XXX | xx | xx | x | 00 | x/xx |
| LDAI | 0/1 | 0/0 | 0/1 | 0/0 | 0/0 | 0/0 | XXX | 0/0 | 1/0 | 0 | 00 | x/xx |
| LDBI | 0/1 | 0/0 | 0/0 | 0/1 | 0/0 | 0/0 | XXX | 0/0 | 0/1 | 0 | 00 | x/xx |
| LUI  | 0/1 | 0/0 | 0/1 | 1/0 | 0/0 | 0/0 | 001 | 0/0 | 0/0 | 0 | 10 | 1/00 |
| ANDI | 0/1 | 0/0 | 0/1 | 0/0 | 0/1 | 0/1 | 000 | x/x | 1/0 | x | 10 | 0/01 |
| DECA | 0/1 | 0/0 | 0/1 | 0/0 | 0/1 | 0/1 | 110 | x/x | 1/0 | x | 10 | 0/10 |
| ADD  | 0/1 | 0/0 | 0/1 | 0/1 | 0/1 | 0/1 | 010 | x/x | 1/1 | x | 10 | 0/00 |
| SUB  | 0/1 | 0/0 | 0/1 | 0/1 | 0/1 | 0/1 | 110 | x/x | 1/1 | x | 10 | 0/00 |
| INCA | 0/1 | 0/0 | 0/1 | 0/0 | 0/1 | 0/1 | 010 | x/x | 1/0 | x | 10 | 0/10 |
| AND  | 0/1 | 0/0 | 0/1 | 0/1 | 0/1 | 0/1 | 000 | x/x | 0/0 | x | 10 | 0/00 |
| ADDI | 0/1 | 0/0 | 0/1 | 0/0 | 0/1 | 0/1 | 010 | x/x | 1/0 | x | 10 | 0/01 |
| ORI  | 0/1 | 0/0 | 0/1 | 0/0 | 0/1 | 0/1 | 001 | x/x | 1/0 | x | 10 | 0/01 |
| ROL  | 0/1 | 0/0 | 0/1 | 0/0 | 0/1 | 0/1 | 100 | x/x | 1/0 | x | 10 | 0/00 |
| ROR  | 0/1 | 0/0 | 0/1 | 0/0 | 0/1 | 0/1 | 101 | x/x | 1/0 | x | 10 | 0/00 |
| CLRA | 0/1 | 0/0 | 1/0 | 0/0 | xx | xx | XXX | x/x | 1/0 | x | xx | x/xx |
| CLRB | 0/1 | 0/0 | 0/0 | 1/0 | xx | xx | XXX | x/x | 0/1 | x | xx | x/xx |
| CLRC | 0/0 | 0/0 | 0/0 | 0/0 | 1/0 | 0/0 | XXX | x/x | x/x | x | xx | x/xx |
| CLRZ | 0/0 | 0/0 | 0/0 | 0/0 | 0/0 | 1/0 | XXX | x/x | x/x | x | xx | x/xx |
| PC <= PC+4 | 0/0 | 1/1 | 0/0 | 0/0 | 0/0 | 0/0 | XXX | x/x | x/x | x | xx | x/xx |
| IR <= M[INST] | 0/1 | 0/0 | 0/0 | 0/0 | 0/0 | 0/0 | XXX | x/x | x/x | x | 00 | x/xx |
| PC <= IR[15..0] | 0/0 | 1/0 | 0/0 | 0/0 | 0/0 | 0/0 | XXX | x/x | x/x | x | xx | x/xx |

## | Waveform Simulations | ![Link](waveforms.pdf) |
