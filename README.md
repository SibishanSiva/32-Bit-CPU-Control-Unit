# 32-Bit CPU Control Unit in VHDL

This project is a VHDL-based Control Unit designed to drive the data-path of a 32-bit CPU, implemented and simulated using Quartus II on an Altera FPGA. The control unit decodes a 32-bit instruction register, manages a 3-state FSM, and generates all control signals needed for the registers, ALU, and MUXes in the CPU data-path.

## Features
* 3-state Moore FSM (T0, T1, T2) that sequences instruction execution, with an enable/reset input to pause and resume operation.
* Operation Decoder that reads the opcode (INST[31..28]) and function code (INST[27..24]) alongside status bits (Carry and Zero) to correctly set all data-path control signals using case statements.
* Memory Signal Generator that handles Write Enable (wen) and Enable (en) signals with correct setup and hold timing for load and store memory operations, sensitive to both clk and mclk.
* Supports 20+ instructions including LDAI, LDBI, LDA, LDB, STA, STB, LUI, JMP, BEQ, BNE, ADD, ADDI, SUB, INCA, DECA, ROL, ROR, AND, ANDI, ORI, CLRA, CLRB, CLRC, CLRZ, TSTZ, and TSTC.
* Conditional branching support via TSTZ and TSTC, which increment the PC only when the corresponding status flag is set.
* Full functional simulation waveforms verifying correct control signal output for all supported instructions across all three execution states.
