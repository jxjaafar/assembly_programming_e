# 05.asm — Pushing and popping 64-bit values

## Purpose
This program demonstrates how the stack stores full 64-bit values and how the last value pushed is the first value popped. It uses RAX and RBX as the source and destination registers.

## Execution and stack flow
Let `RSP₀` represent the value of RSP when `_start` begins.

1. The first two `mov` instructions load `0x1122334455667788` into RAX and `0x1234` into RBX. Loading these registers does not change RSP.
2. `push rax` decreases RSP by 8 bytes and stores `0x1122334455667788` at the top of the stack. RSP becomes `RSP₀ - 8`.
3. `push rbx` decreases RSP by another 8 bytes and stores `0x1234` at the new top. RSP becomes `RSP₀ - 16`.
4. The `nop` instruction leaves the stack unchanged. At this point, the top value is `0x1234`, and the value below it is `0x1122334455667788`.
5. `pop rbx` retrieves `0x1234` into RBX and increases RSP by 8 bytes to `RSP₀ - 8`.
6. `pop rax` retrieves `0x1122334455667788` into RAX and increases RSP by another 8 bytes to `RSP₀`.
7. The program then replaces RAX with 60, clears RDI, and exits through the 64-bit syscall interface. These exit instructions do not perform any additional stack pushes or pops.

## Observations
- Each push of a 64-bit register decreases RSP by 8 bytes; each pop increases it by 8 bytes.
- Before the pop instructions, the most recently pushed value, `0x1234`, is at the top of the stack.
- The values are restored in reverse order: RBX receives `0x1234`, then RAX receives `0x1122334455667788`.
- After both pops, RSP returns to `RSP₀`. RAX is subsequently changed to the exit syscall number, 60, and the program exits with status 0.

## Build and run
```bash
nasm -f elf64 05.asm -o 05.o
ld 05.o -o 05
./05
echo $?
```