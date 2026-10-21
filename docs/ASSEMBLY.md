# BASM Assembly Layer

The machine layer provides a structured assembly dialect embedded in `.basm` files.

## x86-64 example

```basm
asm
    mov rax, 10
    mov rbx, 20
    add rax, rbx
end
```

## Labels

```basm
asm
start:
    cmp rax, 0
    je done
    dec rax
    jmp start
done:
    ret
end
```

## Register constraints

Assembly functions can declare explicit input/output mappings:

```basm
asm_function add(a: i64, b: i64): i64
    input a -> rdi
    input b -> rsi
    output rax
    add rdi, rsi
    mov rax, rdi
    ret
end
```

## Safety model

Normal BASM code is type-checked. Raw assembly is explicitly marked and may bypass memory safety. The compiler should diagnose unknown registers, malformed operands, invalid instructions, and incompatible symbolic operands before backend emission.

## Architecture

The first assembler target is x86-64. The compiler architecture keeps instruction selection and register definitions behind a target interface so ARM64 and RISC-V can be added without changing the high-level grammar.
