# The _Stupid Poorly Engineered Application Runner_

A very simple CPU and the target platform of the [Large Headache Compiler](https://github.com/Ryz3D/LHC). This repository contains all hardware plans and an implementation in Logisim.

## Features

- 16-bit instruction register
  - Up to 65k instructions
- 8-bit control word with 8-bit literal
- Two 8-bit ALU registers (A and B)
  - Operations:
  - ADD: Outputs sum of A and B
  - INV: Outputs two's complement of A
- 8-bit RAM pointer register
  - 256-byte RAM
- Jump by writing target address to RAM address 1 and 2
- Conditional jump (A < 0) by writing target address to RAM 3 and 4
- 8-bit hardware output by writing to RAM 7
