# SUB

## sub1.asm

The program subtracts 80 from 50.

50 - 80 = -30

The 8-bit result is E2h.

### Flags

| Flag | Result | Reason |
|---|---|---|
| CF | 1 | A borrow is needed because 50 is smaller than 80. |
| ZF | 0 | The result is not zero. |
| SF | 1 | The most significant bit of E2h is 1. |
| OF | 0 | -30 is within the signed 8-bit range. |
| AF | 0 | There is no borrow between bit 3 and bit 4. |
| PF | 0 | E2h has five 1s, so it has odd parity. |

## sub2.asm

The program subtracts 2000 from 1000.

1000 - 2000 = -1000

The 16-bit result is FC18h.

### Flags

| Flag | Result | Reason |
|---|---|---|
| CF | 1 | A borrow is needed because 1000 is smaller than 2000. |
| ZF | 0 | The result is not zero. |
| SF | 1 | The most significant bit of FC18h is 1. |
| OF | 0 | -1000 is within the signed 16-bit range. |
| AF | 0 | There is no borrow from bit 3 to bit 4. |
| PF | 1 | 18h has two 1s, so it has even parity. |
