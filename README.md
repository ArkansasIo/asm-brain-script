# BrainScript ASM (BASM)

BrainScript ASM is a unified systems/scripting language combining x86-64-style assembly, Lua-like control flow, and object-oriented programming in one source language.

## Goals

- Lua-like readable scripting syntax.
- Classes, objects, inheritance, interfaces, closures, modules, and events.
- Native integer, floating-point, pointer, struct, and memory types.
- Inline assembly as a first-class language construct.
- x86-64 first backend, with ARM64 and RISC-V planned.
- Native FFI and operating-system interfaces.
- A compiler, assembler, runtime, debugger, formatter, and test runner.

## Example

```basm
module Main

class Player
    field name: string
    field hp: int64

    constructor(name: string, hp: int64)
        self.name = name
        self.hp = hp
    end

    method damage(amount: int64)
        self.hp -= amount
    end
end

asm_function fast_add(a: int64, b: int64): int64
    input a -> rdi
    input b -> rsi
    output rax

    add rdi, rsi
    mov rax, rdi
    ret
end

function main()
    local player = Player.new("Knight", 100)
    player:damage(25)
    print(player.hp)
    print(fast_add(20, 22))
end
```

## Toolchain

```text
basmc       BASM compiler
basm        interpreter/runtime
basm-as     assembly frontend
basm-link   linker driver
basm-fmt    formatter
basm-test   test runner
basm-debug  debugger
```

## Repository layout

```text
compiler/      lexer, parser, AST, semantic analysis, IR, optimizer, backends
runtime/       objects, memory, tables, strings, functions, coroutines
assembler/     architecture-specific assembly frontend
std/           standard library
examples/      BASM programs
docs/           language and architecture documentation
tests/          compiler/runtime/assembler tests
```

See `docs/LANGUAGE.md`, `docs/ASSEMBLY.md`, and `docs/OBJECT_MODEL.md` for the initial language specification.

## Status

Early language and compiler bootstrap. The repository is intentionally being built in layers: specification -> lexer/parser -> AST/semantic model -> IR -> x86-64 backend -> runtime -> standard library.
