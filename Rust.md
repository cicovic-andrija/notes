## About Rust

- Compiled, statically typed language.
- Expression-based language.

## Common Programming Concepts

- Variables are immutable by default.
- Use the `let` keyword to declare a variable.
- Use the `mut` keyword to specify that the variable is mutable.
- Use the `const` keyword to declare a constant.
- The type of the constant must be annotated.
- Naming convention: constants are usually named like BUFFER_SIZE.
- Use the `let` keyword to declare a variable that shadows an existing
    variable with the same name.
- Use shadowing to change the type of the value, but reuse the name.
- The type of a variable cannot be changed with assignment (for mutable variables).
- Use the type annotation to specify the type of a variable: `let n: i32 = 42;`.
- Scalar types: integers (two's complement), floating-point numbers (IEEE-754),
    booleans (`true`/`false`) and characters.
- Primitive compound types: tuples and arrays.
- The `isize` and `usize` integer types represent the native word width on the
    target architecture.
- Use `isize` or `usize` for indexing of collections.
- Use a type suffix to specify the type of an integer constant: `57u8`.
- Use the underscore as a visual separator: `1_000_000`.
- Character type `char` is 4 bytes long and represents a Unicode Scalar Value
    (U+0000-U+D7FF or U+E000-U+10FFFF).
- Use tuples for fix-length, variable-type compound values: `let x: (i32, f64) = (42, 3.14);`.
- Tuples can be destructured, or accessed by using a `.` (period) with zero-indexing: `x.0`.
- The tuple without any values `()` is a special type called "unit type" with only
    one value `()` called "unit value", returned by expressions if they don't return
    any other value.
- Use arrays for fix-length, same-type compund values: `let a = [1, 2, 3];`.
- Array syntax: specify size explicitly: `let a: [i32, 2] = [1, 2];`,
    same value initialization `42`: `let a = [42; 3]`, indexing: `a[0]`.
- Runtime performs index-out-of-bound checking.
- Programming constructs: statements and expressions. Statements do not return a value.
    Expressions evaluate to a resulting value.
- Expressions do not end with semicolons, otherwise they would be considered
    as statements and would not return a value.
- A new scope block created with `{` and `}` is an expression as well, and evaluates
    to the last expression in the block.
- Use the `fn` statement to declare a function.
- The `main` function is the program entry point.
- Naming convention: functions are named using snake_case.
- Function parameters and return type must have type annotations:
    `fn f(a: i32, c: char) -> i32 {}`.
- Use the `if`/`else` expression for conditional branching: `let n = if b { 1 } else { 2 };`.
- Use the `loop` (infinite), `while` (conditional) and `for` (iteration) keywords for
    various kinds of loops.
- Use the `break` statement to break out of the (innermost) loop.
- Use the  `continue` statement to jump to the beginning of the (innermost) loop.
- Use labeled loops for more complex control flow: `'label_name: loop { break 'label_name; }`.
- Use the `break` statement to return a value out of the loop: `let a = loop { break 3; }`.
- Use the `for` loop for iterating a collection: `for elem in collection {}`.
- Use `//` to begin a comment that ends at the end of the line.

## Ownership

## Structs

## Enums and Pattern Matching

## Packages, Crates, and Modules

## Collections

## Error Handling

## Generic Types, Traits, and Lifetimes

## Automated Tests

## Input/Output

## Iterators and Closures

## Package Management with Cargo

## Smart Pointers

## Concurrency

## OOP Features

## Patterns and Matching
