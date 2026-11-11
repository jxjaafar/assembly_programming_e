# 03.asm — CALL and RET (64-bit)

## Purpose
This program demonstrates that function calls use the stack even when the function does not contain explicit `push` or `pop` instructions. The `call` instruction saves a return address, and `ret` retrieves it.

## Execution and stack flow
Let `RSP₀` represent the value of RSP when `_start` begins.

1. `mov rax, 10` places 10 in RAX. RSP is not affected.
2. `call calculate` pushes the address labelled `after_call` onto the stack and transfers execution to `calculate`. Because this is a 64-bit program, RSP decreases by 8 bytes to `RSP₀ - 8`.
3. `add rax, 5` adds 5 to the value in RAX. RAX changes from 10 to 15, while RSP remains at `RSP₀ - 8`.
4. `ret` takes the saved return address from the top of the stack, transfers execution to `after_call`, and increases RSP by 8 bytes. RSP is restored to `RSP₀`.
5. At `after_call`, RAX contains 15. The `nop` instruction does nothing, and the program then exits using syscall 60 with RDI set to zero.

## Observations
- The stack is used even though the function has no explicit push or pop instruction. `call` automatically pushes the return address.
- While `calculate` is running, RSP is 8 bytes below its value at the start of `_start`.
- `ret` removes the return address and restores RSP to `RSP₀`, allowing execution to continue at `after_call`.
- The calculation leaves 15 in RAX. The program exits with status 0.

## Build and run
```bash
nasm -f elf64 03.asm -o 03.o
ld 03.o -o 03
./03
echo $?
```