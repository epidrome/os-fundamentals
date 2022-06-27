---
layout: default
---

## Assembly language

The command architecture of a processor is designed around the following axes:

### Registers

- Data registers

     - EAX (AX (AH/AL)): accumulator

     - EBX (BX (BH/BL)): base memory

    - ECX (DX (CH/CL)): counter

    - EDX (DX (DH/DL)): multiplication / division

- Address registers

    - SP: (stack pointer)

    - BP: (base pointer) 

    - SI: (source index) 

    - DI: (desination index) 

 ![registers](/assets/images/regs.png)  


- Memory segment registers

    - CS: (code segment) 

    - DS: (data segment) 

    - SS: (stack segment) 

    - ES: (extra segment) 

    - FS: (extra segment) 

    - GS: (extra segment) 

- Control registers

    - IP: (instruction pointer) 

    - FLAGS: (Flags)

### Define registers

- registers content: AX
- (Immediate operand): 1234
- (Direct address): [1234]
- (Register indirect address): [BX]
- (Based address): [1234 + BX]
- (Indexed address): [1234 + SI]
- (Based indexed address): [1234 + BX + SI]

### Arithmetic operations

- ( ADD AX, BX)
- ( SUB CX, 3)
- ( ADD DX, [54 + SI])
- ( SUB [54 + SI], AL)
- ( MOV [12 + BP + SI], 45)

### Branching Indicators

- CF: (Carry flag) 
- ZF: (Zero flag)
- SF: (Sign flag)
- PF: (Parity flag)
- OF: (Overflow flag)

![flags](/assets/images/flags.png)

### Basic commands

- MOV destination, source

        (Move)
        destination <- source
- PUSH source

        Move to stuck
        SP <- SP - 2
        [SP] <- source
- POP destination    

        Move from stuck
        destination <- [SP]
        SP <- SP + 2
- XCHG destination, source

        (Exchange)
        destination <- source
        source <- destination
        (simultaneously)

- ADD destination, source

        (Add)
        destination <- destination + source
- INC destination

        (Increment)
        destination <- destination + 1
- SUB destination, source

        (Subtract)
        destination <- destination - source
- DEC destination

        (Decrement)
        destination <- destination - 1
- NEG destination 

        (Negate)
        destination <- - destination
- CMP destination, source 

        (Compare)
        The destination-source operation is performed and the branching indicators are updated.
- MUL source 

        (Multiply)
        DX:AX <- AX * source
- DIV source

        (Divide)
        AX <- AX / source
        DX <- AX mod source

### Binary operations

- AND destination, source

        destination <- destination & source
- OR destination, source
        
        destination <- destination | source
- XOR destination, source

        destination <- destination ^ source
- NOT destination

        destination <- ~ destination
- TEST destination, source

        SHL destination, number
        (Shift left)
        destination <- destination << source
- SHR destination, number

        (Shift right)
        destination <- destination >> source
- SAR destination, number

        (Shift arithmetic right)
        destination <- destination >> source
- ROL destination, number

        (Rotate left)
- ROR destination, number

        (Rotate right)

### Jump commands

- JMP destination

        Jump
        CALL destination
- RET

        (Return)
- JA/JNBE destination

        (Jump above)
- JAE/JNB destination

        (Jump above or equal)
- JB/JNAE destination

        (Jump bellow)
- JBE/JNA destination

        (Jump below or equal)
- JC destination

        (Jump carry)
- JNC destination

        (Jump no carry)
- JE/JZ destination

        (Jump equal)
- JNE/JNZ destination

        (Jump not equal)
- JG/JNLE destination

        (Jump greater)
- JGE/JNL destination

        (Jump greater or equal)
- JL/JNGE destination

        (Jump less)
- JLE/JNG destination

        (Jump less or equal)
- JO destination

        (Jump overflow)
- JNO destination

        (Jump no overflow)
- JS destination

        (Jump sign)
- JNS destination

        (Jump no sign)

[back](./)