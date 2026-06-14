# Brainfuck Calculator

A tiny calculator written in **Brainfuck**, the programming language where every
feature feels like it was smuggled through eight symbols and a lot of pointer
movement.

This started as a simple experiment, then slowly became a real little terminal
calculator with a menu, input handling, and three arithmetic paths. The latest
version is now focused on the core operations that actually made it through the
Brainfuck pain barrier.

## What it can do

The current build supports:

- **Addition**
- **Subtraction**
- **Multiplication**

Division is intentionally not part of the current version.

## Files

| File | Purpose |
| --- | --- |
| `main.bf` | The Brainfuck source code for the calculator. |
| `a.out` | A compiled Linux executable that runs the current calculator build. |
| `README.md` | This project overview and usage guide. |

## Running it

If you are on Linux, you can run the included executable:

```bash
./a.out
```

The program prints the operation menu:

```text
1)ADDING
2)SUBTRACT
3)MULTIPLY
```

Then choose an operation by typing `1`, `2`, or `3`.

## Input notes

This is still Brainfuck, so input is very raw. The program reads characters
directly from standard input instead of using a friendly parser.

For the current version:

- Use **single-digit numbers**.
- Multiplication is the most polished path and prints a normal decimal result.
- Addition and subtraction are still character-output based, so some results may
  display as ASCII characters instead of clean decimal text.

Example multiplication input:

```bash
printf '345' | ./a.out
```

This selects option `3`, then multiplies `4` by `5`.

Expected ending output:

```text
20
```

## Development status

Latest version status:

- Menu UI is complete.
- Addition path exists.
- Subtraction path exists.
- Multiplication was fixed and now works as the main polished feature.
- The repo is considered finished for now.
- Division is not planned for this version.

## Why Brainfuck?

Because making a calculator in a normal language is reasonable, and this project
is not trying to be reasonable.

Brainfuck only gives you:

```text
> < + - . , [ ]
```

So every prompt, number conversion, operation, and printed result has to be built
out of memory cells, pointer movement, loops, and patience.

## Project goal

The goal is not to be the best calculator. The goal is to prove that a calculator
can exist inside Brainfuck at all, while keeping the source readable enough that
future-me has at least a small chance of remembering what happened.
