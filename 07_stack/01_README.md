# 01.asm — Basic push and pop (64-bit)

## Purpose
This program demonstrates how the stack stores values and returns them in last-in, first-out (LIFO) order. It uses 64-bit registers, so each push or pop changes RSP by 8 bytes.

## Execution and stack flow
Let `RSP₀` represent the value of RSP when `_start` begins.

1. `mov rax, 100` and `mov rbx, 200` place the two values in registers. These instructions do not use the stack, so RSP remains at `RSP₀`.
2. `push rax` moves RSP down by 8 bytes and stores 100 at the new top of the stack. RSP becomes `RSP₀ - 8`.
3. `push rbx` moves RSP down by another 8 bytes and stores 200. RSP becomes `RSP₀ - 16`. The value 200 is now above 100 and is the first value that will be removed.
4. The first `nop` leaves the machine state unchanged. At this point, the stack contains 200 at its top, followed by 100.
5. `pop rcx` copies the top value, 200, into RCX and moves RSP up by 8 bytes. RSP becomes `RSP₀ - 8`.
6. `pop rdx` copies the next value, 100, into RDX and moves RSP up by another 8 bytes. RSP returns to `RSP₀`.
7. The second `nop` makes no changes. The program then exits with syscall 60 after clearing RDI.

## Observations
- Each 64-bit push decreases RSP by 8 bytes; each pop increases it by 8 bytes.
- After both pushes, RSP is 16 bytes below its starting value, and the stack order is 200 followed by 100.
- The pop order is the reverse of the push order: RCX receives 200 and RDX receives 100.
- Once both values have been popped, RSP is back at `RSP₀`. The program exits with status 0 because RDI is set to zero before the exit syscall.

## Build and run
```bash
nasm -f elf64 01.asm -o 01.o
ld 01.o -o 01
./01
echo $?
```