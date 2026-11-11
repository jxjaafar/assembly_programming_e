# 06.asm — Pushing and popping 32-bit values

## Purpose
This program demonstrates stack operations with 32-bit registers. It shows that each push stores a 4-byte value and each pop retrieves the most recently stored value.

## Execution and stack flow
Let `ESP₀` represent the value of ESP when `_start` begins.

1. The first two `mov` instructions load `0x11223344` into EAX and `0x1234` into EBX. These instructions do not change ESP.
2. `push eax` decreases ESP by 4 bytes and stores `0x11223344` at the top of the stack. ESP becomes `ESP₀ - 4`.
3. `push ebx` decreases ESP by another 4 bytes and stores `0x1234` at the new top. ESP becomes `ESP₀ - 8`.
4. The `nop` instruction does nothing. The top of the stack now contains `0x1234`, with `0x11223344` below it.
5. `pop ebx` retrieves `0x1234` into EBX and increases ESP by 4 bytes to `ESP₀ - 4`.
6. `pop eax` retrieves `0x11223344` into EAX and increases ESP by another 4 bytes to `ESP₀`.
7. The program sets EAX to 1, clears EBX, and exits through the 32-bit Linux system-call interface using `int 0x80`. No further stack operations occur during this exit sequence.

## Observations
- Each 32-bit push decreases ESP by 4 bytes, and each pop increases ESP by 4 bytes.
- After both pushes, ESP is 8 bytes below its starting value. The top value is `0x1234`, followed by `0x11223344`.
- The pop instructions restore the values in reverse order: EBX receives `0x1234`, and EAX receives `0x11223344`.
- After both pops, ESP returns to `ESP₀`. EAX is then overwritten with the exit system-call number 1, and EBX is set to zero, so the program exits with status 0.

## Build and run
```bash
nasm -f elf32 06.asm -o 06.o
ld -m elf_i386 06.o -o 06
./06
echo $?
```