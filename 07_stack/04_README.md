# 04.asm — Stack frame and local variables (32-bit)

## Purpose
This program demonstrates a function stack frame in 32-bit assembly. It uses ESP as the stack pointer and EBP as the frame pointer. A 32-bit push, pop, call return address, or return address removed by `ret` occupies 4 bytes.

## Execution and stack flow
Let `ESP₀` represent the value of ESP when `_start` begins.

1. `call work` pushes the address of the instruction after the call onto the stack. ESP decreases by 4 bytes to `ESP₀ - 4`.
2. `push ebp` saves the caller value of EBP and decreases ESP by another 4 bytes to `ESP₀ - 8`.
3. `mov ebp, esp` establishes the frame pointer for `work`. ESP does not change.
4. `sub esp, 8` reserves 8 bytes for two 4-byte local variables. ESP becomes `ESP₀ - 16`, which is 8 bytes below EBP.
5. The program stores 100 at `[ebp - 4]` and 200 at `[ebp - 8]`. These memory writes do not change ESP.
6. The values are read into EAX and EBX. EAX receives 100 and EBX receives 200, with ESP remaining unchanged.
7. `mov esp, ebp` releases the local-variable area and restores ESP to `ESP₀ - 8`.
8. `pop ebp` restores the caller frame pointer and increases ESP to `ESP₀ - 4`.
9. `ret` removes the saved return address, increases ESP to `ESP₀`, and returns execution to `_start`.
10. The program exits through the 32-bit Linux system-call interface using `int 0x80`, with EBX set to zero as the exit status.

## Observations
- The 32-bit `call` instruction pushes a 4-byte return address. Saving EBP uses another 4 bytes.
- The function reserves 8 bytes for its two local variables. While this area is in use, ESP is 8 bytes below EBP.
- The local stack slots contain 100 and 200, and the program loads them into EAX and EBX, respectively.
- The function releases the local area, restores EBP, and returns. ESP is back at `ESP₀` after `ret`.
- The exit status is 0 because EBX is cleared before `int 0x80` invokes the exit system call.

## Build and run
```bash
nasm -f elf32 04.asm -o 04.o
ld -m elf_i386 04.o -o 04
./04
echo $?
```