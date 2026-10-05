# MUL

## mul1.asm

The program multiplies 25 by 10.

25 × 10 = 250

The result is stored in AX as 00FAh.

### Flags

| Flag | Result | Reason |
|---|---|---|
| CF | 0 | The upper byte of AX is 00h, so the upper part of the result is zero. |
| OF | 0 | The upper byte of AX is zero, so the result fits in the lower 8 bits. |
| SF | Undefined | MUL does not define the Sign Flag. |
| ZF | Undefined | MUL does not define the Zero Flag. |
| AF | Undefined | MUL does not define the Auxiliary Carry Flag. |
| PF | Undefined | MUL does not define the Parity Flag. |

## mul2.asm

The program multiplies 3000 by 200.

3000 × 200 = 600000

The result is stored in DX:AX as 0009:27C0h.

### Flags

| Flag | Result | Reason |
|---|---|---|
| CF | 1 | DX is 0009h, so the upper part of the result is not zero. |
| OF | 1 | The result does not fit completely in AX, so the upper half is non-zero. |
| SF | Undefined | MUL does not define the Sign Flag. |
| ZF | Undefined | MUL does not define the Zero Flag. |
| AF | Undefined | MUL does not define the Auxiliary Carry Flag. |
| PF | Undefined | MUL does not define the Parity Flag. |
