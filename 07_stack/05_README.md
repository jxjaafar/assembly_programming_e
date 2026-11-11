# 05.asm — Pushing and popping 64-bit values

## Purpose
This program demonstrates that the stack can hold full 64-bit register values and that values are popped in the reverse order in which they were pushed.

## Execution and stack flow
Let `RSP₀` be RSP at program entry.

1. `mov rax, 0x1122334455667788` and `mov rbx, 0x1234` load two values into registers. The stack is unchanged.
2. `push rax` decreases RSP by 8 and stores `0x1122334455667788) at the top of the stack.
3. `push rbx` decreases RSP by another 8 and stores `0x1234) above the first value. RSP is now `RSP₀ - 16`.
4. `pop rbx` retrieves the most recently pushed value, `0x1234), into RBX and increases RSP by 8.
5. `pop rax` retrieves `0x1122334455667788) into RAX and increases RSP by 8. RSP returns to `RSP₀`.
6. The program then overwrites RAX with 60 for the exit syscall, clears RDI, and exits.

## Expected observations when tested
- Before the pops, the top of the stack is `0x1234), followed by `0x1122334455667788).
- After the pops, RBX and RAX hold their original values and RSP is restored.
- RAX is subsequently changed to the syscall number 60; this does not affect the stack. The expected exit status is 0.

## Build and run
```bash
nasm -f elf64 05.asm -o 05.o
ld 05.o -o 05
./05
echo $?
```

*Testing note: this explanation is a source-based stack trace. The program has not been executed in this environment, so the observations above are expected results, not recorded debugger output.*
