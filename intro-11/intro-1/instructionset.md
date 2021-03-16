# .instructionset

We've now reversed all the VM instructions, and have a full understanding about how it works. Here is the VM's instruction set:

| Instruction | 1st arg | 2nd arg | What does it do? |
| :--- | :--- | :--- | :--- |
| "A" | "M" | arg2 | \*sym.current\_memory\_ptr += arg2 |
|  | "P" | arg2 | sym.current\_memory\_ptr += arg2 |
|  | "C" | arg2 | sym.written\_by\_instr\_C += arg2 |
| "S" | "M" | arg2 | \*sym.current\_memory\_ptr -= arg2 |
|  | "P" | arg2 | sym.current\_memory\_ptr -= arg2 |
|  | "C" | arg2 | sym.written\_by\_instr\_C -= arg2 |
| "I" | arg1 | n/a | instr\_A\(arg1, 1\) |
| "D" | arg1 | n/a | instr\_S\(arg1, 1\) |
| "P" | arg1 | n/a | \*sym.current\_memory\_ptr = arg1; instr\_I\("P"\) |
| "X" | arg1 | n/a | \*sym.current\_memory\_ptr ^= arg1 |
| "J" | arg1 | n/a | arg1\_and\_0x3f = arg1 & 0x3f; if \(arg1 & 0x40 != 0\)   arg1\_and\_0x3f \*= -1 if \(arg1 &gt;= 0\) return arg1\_and\_0x3f; else if \(\*sym.written\_by\_instr\_C != 0\) {   if \(arg1\_and\_0x3f &lt; 0\)     ++\*sym.good\_if\_ne\_zero;   return arg1\_and\_0x3f; } else return 2; |
| "C" | arg1 | n/a | \*sym.written\_by\_instr\_C = arg1 |
| "R" | arg1 | n/a | return\(arg1\) |

