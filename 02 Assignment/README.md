Virtual CPU
===========

The virtual CPU has four registers, one flag, and a memory of 64 bytes.
Instructions are all 8 bit.
The CPU is optimised for stack operations,
there are no instructions for direct memory access.

The virtual cpu can be run with
```shell
$ java -jar VirtualCPU.jar
```

Registers and flag:
-------------------

| Name  | Type                | Description                                  |
| :---: | :------------------ | :------------------------------------------- |
| A     | Accumulator         | Primary accumulator                          |
| B     | Accumulator         | Secondary accumulator                        |
| SP    | Stack Pointer       | Points to last element pushed                |
| IP    | Instruction Pointer | Points to next instruction                   |
| F     | Flag                | Decides if next `JMP` or `CALL` is effective |

Arguments:
----------

| Abr.  | Type        | Bits  | Literal                      |
| :---: |:----------- | :---: | :--------------------------- |
| r     | Register    | 1     | `A` (= 0) or `B` (= 1)       |
| o     | Offset      | 3     | `+0` to `+7` Offset to SP    |
| v     | Value       | 5     | `-16` to `15` (2-complement) |
| a     | Address     | 6     | `#0` to `#63`                |


Instructions:
-------------

| Binary      | Code      | Function                                           |
| :---------: | :-------- | :------------------------------------------------- |
| `0000 0000` | `NOP`     | IP++                                               |
| `0000 0001` | `ADD`     | A ← A + B; IP++                                    |
| `0000 0010` | `MUL`     | A ← A*B; IP++                                      |
| `0000 0011` | `DIV`     | A ← A/B; IP++                                      |
| `0000 0100` | `ZERO`    | F ← A = 0; IP++                                    |
| `0000 0101` | `NEG`     | F ← A < 0; IP++                                    |
| `0000 0110` | `POS`     | F ← A > 0; IP++                                    |
| `0000 0111` | `NZERO`   | F ← A ≠ 0; IP++                                    |
| `0000 1000` | `EQ`      | F ← A = B; IP++                                    |
| `0000 1001` | `LT`      | F ← A < B; IP++                                    |
| `0000 1010` | `GT`      | F ← A > B; IP++                                    |
| `0000 1011` | `NEQ`     | F ← A ≠ B; IP++                                    |
| `0000 1100` | `ALWAYS`  | F ← **true**; IP++                                 |
| `0000 1101` |           | *Undefined*                                        |
| `0000 1110` |           | *Undefined*                                        |
| `0000 1111` | `HALT`    | *Halts execution*                                  |
| `0001 000r` | `PUSH r`  | [--SP] ← r; IP++                                   |
| `0001 001r` | `POP r`   | r ← [SP++]; IP++                                   |
| `0001 0100` | `MOV A B` | B ← A; IP++                                        |
| `0001 0101` | `MOV B A` | A ← B; IP++                                        |
| `0001 0110` | `INC`     | A++; IP++                                          |
| `0001 0111` | `DEC`     | A--; IP++                                          |
| `0001 1ooo` | `RTN +o`  | IP ← [SP++]; SP += o; IP++                         |
| `0010 rooo` | `MOV r o` | [SP + o] ← r; IP++                                 |
| `0011 ooor` | `MOV o r` | r ← [SP + o]; IP++                                 |
| `01vv vvvr` | `MOV v r` | r ← v; IP++                                        |
| `10aa aaaa` | `JMP #a`  | **if** F **then** IP ← a **else** IP++             |
| `11aa aaaa` | `CALL #a` | **if** F **then** [--SP] ← IP; IP ← a **else** IP++|

#### Notes

The `#a` notation for static addresses is mainly used in output,
in code, labels are preferred:
```
MAIN:      MOV 5 A
           PUSH A
           ALWAYS
           CALL FACT
           POP A
           HALT
FACT:      MOV +1 A
           NZERO
           JMP RECUR
           MOV 1 A
           MOV A +1
           RTN
RECUR:     PUSH A
           DEC
           PUSH A
           ALWAYS
           CALL FACT
           POP B
           POP A
           MUL
           MOV A +1
           RTN +0
```
`RTN` is a short form for `RTN +0`

Considerations:
---------------
There are quite many conditionals, introducing a `NOT` (F ← !F) might
make it possible to reduce the conditionals to `EQ`, `LT`, `GT`,
and `ZERO` or something similar. Three bits for the offset,
allowing 7 arguments, might be over the top.
Reducing it to two bits, would allow for some other instructions.