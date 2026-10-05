# DIV

## div1.asm

The program divides 100 by 7.

100 ÷ 7 = 14 remainder 2

The quotient is stored in AL and the remainder is stored in AH.

### Flags

| Flag | Result | Reason |
|---|---|---|
| CF | Undefined | DIV does not define the Carry Flag. |
| ZF | Undefined | DIV does not define the Zero Flag. |
| SF | Undefined | DIV does not define the Sign Flag. |
| OF | Undefined | DIV does not define the Overflow Flag. |
| AF | Undefined | DIV does not define the Auxiliary Carry Flag. |
| PF | Undefined | DIV does not define the Parity Flag. |

## div2.asm

The program divides 50000 by 300.

50000 ÷ 300 = 166 remainder 200

The quotient is stored in AX and the remainder is stored in DX.

### Flags

| Flag | Result | Reason |
|---|---|---|
| CF | Undefined | DIV does not define the Carry Flag. |
| ZF | Undefined | DIV does not define the Zero Flag. |
| SF | Undefined | DIV does not define the Sign Flag. |
| OF | Undefined | DIV does not define the Overflow Flag. |
| AF | Undefined | DIV does not define the Auxiliary Carry Flag. |
| PF | Undefined | DIV does not define the Parity Flag. |

The flags are undefined because DIV does not specify meaningful values for these flags.
