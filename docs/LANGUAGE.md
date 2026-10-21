# BrainScript ASM Language Specification

## 1. Design

BASM has three coordinated layers:

1. **Script layer** — Lua-like functions, tables, loops, closures, modules, and dynamic values.
2. **Object/system layer** — classes, structs, interfaces, generics, pointers, typed fields, and explicit memory.
3. **Machine layer** — inline assembly, registers, labels, calling conventions, SIMD, atomics, and system calls.

The layers share one parser and semantic model. Assembly is not a separate source language.

## 2. Lexical rules

Identifiers use `[A-Za-z_][A-Za-z0-9_]*`. Strings use double quotes. Comments use `--` for line comments and `--[[ ... ]]` for block comments.

## 3. Statements

```basm
if condition then ... end
while condition do ... end
for i = 0, 10 do ... end
for key, value in pairs(table) do ... end
match value
    case 1 ...
    default ...
end
return value
break
continue
```

## 4. Functions

```basm
function add(a: int64, b: int64): int64
    return a + b
end
```

## 5. Objects

```basm
class Player extends Entity implements Damageable
    field hp: int64

    constructor(hp: int64)
        self.hp = hp
    end

    method damage(amount: int64)
        self.hp -= amount
    end
end
```

## 6. Assembly blocks

```basm
asm
    mov rax, 42
    ret
end
```

An `asm` block has access to validated locals and symbols. The compiler records register clobbers and ABI requirements.

## 7. Explicit assembly functions

```basm
asm_function add(a: int64, b: int64): int64
    input a -> rdi
    input b -> rsi
    output rax
    add rdi, rsi
    mov rax, rdi
    ret
end
```

## 8. Types

Built-in types include `bool`, `i8`, `i16`, `i32`, `i64`, `u8`, `u16`, `u32`, `u64`, `f32`, `f64`, `char`, `string`, `pointer<T>`, `array<T>`, `table`, and `void`.

Aliases `int`, `uint`, and `usize` resolve according to the target ABI.

## 9. Modules

```basm
module Game.Player
import System.Console
export Player
```

## 10. Errors

The initial implementation uses structured runtime errors and compiler diagnostics. Exceptions are represented by `try`, `catch`, and `throw` in the high-level runtime; low-level functions may use result values for zero-overhead error paths.

## 11. ABI

The x86-64 System V ABI is the initial Unix-like target. A Windows x86-64 ABI backend is planned. Calling convention metadata belongs to the function type so FFI and inline assembly can be checked.
