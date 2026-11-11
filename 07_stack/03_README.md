# 03.asm — CALL and RET (64-bit)

## Purpose
This program shows that a function call uses the stack even when the function contains no explicit `push` or `pop` instructions.

## Execution and stack flow
Let `RSP₀` be RSP at the start of `_start`.

1. `mov rax, 10` places 10 in RAX. RSP is unchanged.
2. `call calculate` automatically pushes the address labelled `after_call` onto the stack and transfers execution to `calculate`. RSP decreases by 8 to `RSP₀ - 8`.
3. `add rax, 5` changes RAX from 10 to 15. It does not use or change the stack.
4. `ret` takes the saved return address from the top of the stack, transfers execution to `after_call`, and increases RSP by 8 back to `RSP₀`.
5. At `after_call`, RAX should contain 15. The `nop` does nothing.
6. The program exits using syscall 60.

There is no explicit `push` or `pop) in this program. The important stack activity is performed automatically by `call` and `ret`.

## Expected observations when tested
- Immediately after `call calculate`, RSP is 8 bytes lower and the top stack item is the return address.
- At `after_call`, RSP is restored and RAX = 15.
- The exit code should be 0.

## Build and run
```bash
nasm -f elf64 03.asm -o 03.o
ld 03.o -o 03
./03
echo $?
```

*Testing note: this explanation is a source-based stack trace. The program has not been executed in this environment, so the observations above are expected results, not recorded debugger output.*
