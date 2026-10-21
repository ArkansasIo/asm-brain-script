# BASM Object Model

BASM supports classes for behavior and structs for predictable data layout.

## Classes

A class instance contains an object header followed by implementation-defined fields. The header may contain type metadata, flags, and runtime management information.

```basm
class Entity
    field id: u64
    field active: bool

    method update(dt: f64)
    end
end
```

## Constructors

`constructor` initializes a newly allocated object. `Class.new(...)` is the canonical allocation form.

## Inheritance

Single implementation inheritance is the baseline model. Interfaces provide multiple behavioral contracts without multiple object-layout inheritance.

## Interfaces

```basm
interface Drawable
    method draw()
end
```

Classes implementing an interface must provide its required methods.

## Structs

Structs are value-like aggregates with deterministic field layout and are suitable for FFI and assembly.

```basm
struct Vec3
    x: f64
    y: f64
    z: f64
end
```

## Methods

`self` is implicit in method bodies. `object:method(args)` is syntactic sugar for a method dispatch operation.

## Generics

Generic declarations use angle brackets:

```basm
class Array<T>
    method get(index: usize): T
    end
end
```

The first compiler implementation may specialize generics during lowering.

## Assembly interoperability

A method may contain `asm` and use fields as symbolic operands. The compiler resolves object-field offsets before emitting machine code.
