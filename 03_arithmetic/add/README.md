# ADD

## add1.asm

The program adds 120 and 10.

120 + 10 = 130

The result is 130, which is 82h in hexadecimal.

### Flags

| Flag | Result | Reason |
|---|---|---|
| CF | 0 | There was no carry outside the 8-bit result. |
| ZF | 0 | The result is not zero. |
| SF | 1 | The most significant bit of 82h is 1. |
| OF | 1 | 130 is bigger than the largest signed 8-bit number, which is 127. |
| AF | 1 | There was a carry from bit 3 to bit 4. |
| PF | 1 | 82h has two 1s, so it has even parity. |

## add2.asm

The program adds 32000 and 500.

32000 + 500 = 32500

The result is 7EF4h.

### Flags

| Flag | Result | Reason |
|---|---|---|
| CF | 0 | There was no carry outside the 16-bit result. |
| ZF | 0 | The result is not zero. |
| SF | 0 | The most significant bit is 0. |
| OF | 0 | 32500 is within the signed 16-bit range. |
| AF | 0 | There was no carry from bit 3 to bit 4. |
| PF | 0 | F4h has five 1s, so it has odd parity. |
