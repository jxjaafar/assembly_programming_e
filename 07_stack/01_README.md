# 01.asm — Basic push and pop (64-bit)

## Purpose
This program demonstrates how values are placed on the stack with `push` and retrieved with `pop`. It uses 64-bit registers, so each register push/pop occupies 8 bytes.

## Execution and stack flow
Let `RSP₀` be the stack pointer at program entry. The exact address depends on the process, so the changes below are relative.

1. `mov rax, 100` and `mov rbx, 200` load the values into registers. These instructions do not change the stack.
2. `push rax` decreases RSP by 8, then stores 100 at the new top of the stack. RSP is now `RSP₀ - 8`.
3. `push rbx` decreases RSP by another 8 and stores 200 above the earlier value. RSP is now `RSP₀ - 16`.
4. `pop rcx` reads the top value (200) into RCX, then increases RSP by 8. RSP becomes `RSP₀ - 8`.
5. `pop rdx` reads the next value (100) into RDX, then increases RSP by 8. RSP returns to `RSP₀`.
6. `nop` does nothing. The program then exits with the Linux x86-64 `syscall` instruction.

The order matters: a stack is last-in, first-out (LIFO). The last value pushed (200) is the first value popped.

## Expected observations when tested
- At the first `nop`: RSP is 16 bytes below its starting value; the top item is 200 and the next item is 100.
- After the two `pop` instructions: RCX = 200, RDX = 100, and RSP has returned to its starting value.
- The exit code should be 0 because RDI is cleared before syscall 60.

## Build and run
```bash
nasm -f elf64 01.asm -o 01.o
ld 01.o -o 01
./01
echo $?
```

*Testing note: this explanation is a source-based stack trace. The program has not been executed in this environment, so the observations above are expected results, not recorded debugger output.*
