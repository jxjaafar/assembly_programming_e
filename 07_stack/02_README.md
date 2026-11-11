# 02.asm — Stack frame and local variables (64-bit)

## Purpose
This program demonstrates how a function uses the stack to save the caller frame pointer, create a stack frame, reserve space for local variables, and restore the stack before returning.

## Execution and stack flow
Let `RSP₀` represent the stack pointer when `_start` begins.

1. `call work` places the address of the instruction immediately after the call on the stack. RSP decreases by 8 bytes to `RSP₀ - 8`. This return address allows the function to resume execution in `_start` when it finishes.
2. At the start of `work`, `push rbp` saves the caller value of RBP. RSP decreases by another 8 bytes to `RSP₀ - 16`.
3. `mov rbp, rsp` sets RBP to the base of the new stack frame. Unlike a push or pop, this instruction does not change RSP.
4. `sub rsp, 16` reserves 16 bytes for local variables. RSP becomes `RSP₀ - 32`, which is 16 bytes below RBP.
5. The program stores 100 at `[rbp - 8]` and 200 at `[rbp - 16]`. These are two 8-byte local-variable slots in the stack frame. Writing to them does not change RSP.
6. The next instructions read the values into RAX and RBX. RAX receives 100 and RBX receives 200; the stack pointer remains unchanged.
7. `mov rsp, rbp` releases the 16 bytes reserved for local variables by moving RSP back to the frame base, `RSP₀ - 16`.
8. `pop rbp` restores the caller frame pointer and increases RSP to `RSP₀ - 8`.
9. `ret` removes the return address from the stack, increases RSP to `RSP₀`, and transfers execution back to the instruction after `call work`.
10. The program exits from `_start` using syscall 60 after setting RDI to zero.

## Observations
- The call instruction itself uses the stack: it pushes an 8-byte return address before `work` begins.
- Saving RBP uses another 8 bytes. The 16-byte local-variable area is then reserved below the frame pointer.
- While the local variables are allocated, RSP is 16 bytes below RBP. The slots at `[rbp - 8]` and `[rbp - 16]` hold 100 and 200, respectively.
- The function restores RSP from RBP, restores the previous RBP, and returns. After `ret`, RSP has its original value from the beginning of `_start`.

## Build and run
```bash
nasm -f elf64 02.asm -o 02.o
ld 02.o -o 02
./02
echo $?
```