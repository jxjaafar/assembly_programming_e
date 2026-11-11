# 02.asm — Stack frame and local variables (64-bit)

## Purpose
This program demonstrates a function stack frame. The function `work` saves the caller's frame pointer, establishes its own frame, reserves 16 bytes for local variables, and then restores the stack before returning.

## Execution and stack flow
Let `RSP₀` be RSP when `_start` begins.

1. `call work` pushes the address of the instruction immediately after the call onto the stack. RSP decreases by 8 to `RSP₀ - 8`; this is the return address.
2. `push rbp` saves the caller's RBP. RSP decreases by another 8 to `RSP₀ - 16`.
3. `mov rbp, rsp` makes RBP the fixed base of this function's stack frame.
4. `sub rsp, 16` reserves 16 bytes for local data. RSP becomes `RSP₀ - 32`. The two qword slots are addressed as `[rbp - 8]` and `[rbp - 16]`.
5. The two `mov` instructions write 100 and 200 into those local stack slots. They do not further change RSP.
6. The next two `mov` instructions read those slots into RAX and RBX. They also leave RSP unchanged.
7. `mov rsp, rbp` discards the 16-byte local-variable area by restoring RSP to the saved frame base (`RSP₀ - 16`).
8. `pop rbp` restores the caller's RBP and increases RSP to `RSP₀ - 8`.
9. `ret` pops the saved return address into the instruction pointer and increases RSP to `RSP₀). Execution resumes after `call work`, and the program exits.

## Expected observations when tested
- While the function's local area is allocated, RSP is 16 bytes below RBP.
- The memory locations `[rbp - 8]` and `[rbp - 16]` contain 100 and 200 respectively; RAX and RBX receive those values.
- After `ret`, the call's return address has been removed and RSP is back to its entry value.

## Build and run
```bash
nasm -f elf64 02.asm -o 02.o
ld 02.o -o 02
./02
echo $?
```

*Testing note: this explanation is a source-based stack trace. The program has not been executed in this environment, so the observations above are expected results, not recorded debugger output.*
