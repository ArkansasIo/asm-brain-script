# BASM Compiler Architecture

```text
.basm source
    |
    v
Lexer -> Parser -> AST
                |
                v
          Semantic Analyzer
                |
                +--> Type / ABI checking
                +--> Object layout
                +--> Assembly validation
                |
                v
             BASM IR
                |
         +------+------+
         |             |
    Optimizer     Assembly IR
         |             |
         +------+------+
                |
                v
          Target Backend
          /      |      \
      x86-64   ARM64   RISC-V
                |
                v
             Object
                |
              Linker
                |
             Executable
```

## Compiler components

- **Lexer:** converts source into tokens and preserves source spans.
- **Parser:** builds a typed AST and recognizes inline assembly blocks.
- **Semantic analyzer:** resolves names, scopes, types, inheritance, interfaces, ABI constraints, and assembly operands.
- **IR:** architecture-neutral representation for high-level code plus explicit machine operations.
- **Optimizer:** constant folding, dead-code elimination, inlining, basic-block optimization, and later register-aware optimization.
- **Backend:** lowers IR into architecture-specific instructions.
- **Runtime:** implements objects, tables, strings, allocation, coroutines, exceptions, and FFI support.

## Bootstrap implementation order

1. Lexer.
2. Parser for declarations, expressions, functions, classes, and `asm` blocks.
3. AST and diagnostics.
4. Interpreter for high-level constructs.
5. Typed IR.
6. x86-64 assembly emission.
7. Native object/linker integration.
8. Runtime object model.
9. FFI and standard library.
10. Debug information and debugger.
