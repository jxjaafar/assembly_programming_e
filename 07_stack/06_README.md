# 06.asm — Pushing and popping 32-bit values

## Purpose
This program is the 32-bit counterpart to the 64-bit push/pop demonstration. It uses EAX and EBX, and each push or pop changes ESP by 4 bytes.

## Execution and stack flow
Let `ESP₀` be ESP at program entry.

1. `mov eax, 0x11223344` and `mov ebx, 0x1234` load the values into registers without changing ESP.
2. `push eax` decreases ESP by 4 and stores `0x11223344) at the top of the stack.
3. `push ebx` decreases ESP by another 4 and stores `0x1234) at the new top. ESP is now `ESP₀ - 8`.
4. `pop ebx` retrieves `0x1234) first and increases ESP by 4.
5. `pop eax` retrieves `0x11223344) next and increases ESP by 4. ESP returns to `ESP₀`.
6. The program then sets EAX to 1, clears EBX, and exits through `int 0x80).

## Expected observations when tested
- Before the pops, the top stack value is `0x1234), followed by `0x11223344).
- After the pops, EAX and EBX contain their original values and ESP has returned to its starting value.
- EAX is then overwritten with the 32-bit exit syscall number 1. The expected exit status is 0.

## Build and run
```bash
nasm -f elf32 06.asm -o 06.o
ld -m elf_i386 06.o -o 06
./06
echo $?
```

*Testing note: this explanation is a source-based stack trace. The program has not been executed in this environment, so the observations above are expected results, not recorded debugger output.*
