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

## Technical

### Control word

ROM1 contains the 8 bit control word (CW), ROM2 contains the 8 bit literal (LIT), both are part of the current instruction. Thus they are always addressed identically.
The CW controls what component writes to the bus (MSB bits 4-7) and which components read from the bus (LSB bits 0-3). If no component is selected to output (CW bits 4-7 are 0), the literal is written to the bus.

From bus into...

- **CW0** (`RAM`): RAM
- **CW1** (`RAM_P`): RAM_POINTER
- **CW2** (`A`): A
- **CW3** (`B`): B

Onto bus from...

- **CW4** (`RAM`): RAM
- **CW5** (`RAM_P`): RAM_POINTER
- **CW6** (`ADD`): Adder (A+B)
- **CW7** (`INV`): Inverter (-A)

### Examples

| Operation   | Assembly     | CW       |
| ----------- | ------------ | -------- |
| RAM = A + B | `ADD -> RAM` | 01000001 |
| A = -A      | `INV -> A`   | 10000100 |
| RAM = B - A | `INV -> A`   | 10000100 |
|             | `ADD -> RAM` | 01000001 |

To use a literal value in assembly code simply write the value on the left side:

| Operation | Assembly     | CW       | LIT      |
| --------- | ------------ | -------- | -------- |
| RAM_P = 8 | `8 -> RAM_P` | 00000010 | 00001000 |
| RAM = 127 | `127 -> RAM` | 00000001 | 01111111 |

### RAM functions

| Address | Usage                             |
| ------- | --------------------------------- |
| 0       | Reserved                          |
| 1       | Prepare jump (8 MSB)              |
| 2       | Jump (8 LSB)                      |
| 3       | Prepare conditional jump (8 MSB)  |
| 4       | Conditional jump if A < 0 (8 LSB) |
| 5       | Return address (8 MSB)            |
| 6       | Return address (8 LSB)            |
| 7       | Hardware I/O                      |

Addresses 8-255 are freely useable.
