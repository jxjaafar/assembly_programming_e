# 04.asm — Stack frame and local variables (32-bit)

## Purpose
This is the 32-bit version of the stack-frame example. It uses EBP as the frame pointer and ESP as the stack pointer. A 32-bit push, pop, call return address, or ret operation uses 4 bytes.

## Execution and stack flow
Let `ESP₀` be ESP when `_start` begins.

1. `call work` pushes the return address. ESP decreases by 4 to `ESP₀ - 4`.
2. `push ebp` saves the caller's EBP and decreases ESP to `ESP₀ - 8`.
3. `mov ebp, esp` establishes the frame pointer for `work`.
4. `sub esp, 8` reserves 8 bytes for two 4-byte local variables. ESP becomes `ESP₀ - 16`.
5. The program stores 100 at `[ebp - 4]` and 200 at `[ebp - 8]`. These writes do not change ESP.
6. It reads the local values into EAX and EBX. These reads do not change ESP.
7. `mov esp, ebp` releases the 8-byte local area and restores ESP to `ESP₀ - 8`.
8. `pop ebp` restores the caller's EBP and increases ESP to `ESP₀ - 4`.
9. `ret` removes the saved return address and increases ESP to `ESP₀`, returning to `_start`. The program then exits using the 32-bit `int 0x80) interface.

## Expected observations when tested
- During the local-variable section, ESP is 8 bytes below EBP.
- The local slots contain 100 and 200; EAX and EBX are loaded with those values.
- After `ret`, ESP is restored to its starting value.

## Build and run
```bash
nasm -f elf32 04.asm -o 04.o
ld -m elf_i386 04.o -o 04
./04
echo $?
```

*Testing note: this explanation is a source-based stack trace. The program has not been executed in this environment, so the observations above are expected results, not recorded debugger output.*
