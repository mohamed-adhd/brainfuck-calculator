<div align="center">

  <h1>Brainfuck Calculator</h1>

  <p>
    <b>A terminal calculator written in Brainfuck, built from raw memory cells, pointer movement, ASCII input, loops, and unreasonable patience.</b>
  </p>

  <p>
    <img alt="Brainfuck" src="https://img.shields.io/badge/Brainfuck-111827?style=for-the-badge" />
    <img alt="Esoteric Language" src="https://img.shields.io/badge/Esoteric_Language-FF006E?style=for-the-badge" />
    <img alt="Terminal" src="https://img.shields.io/badge/Terminal_CLI-00C2A8?style=for-the-badge" />
    <img alt="Memory Tape" src="https://img.shields.io/badge/Memory_Tape-7C3AED?style=for-the-badge" />
    <img alt="ASCII" src="https://img.shields.io/badge/ASCII_IO-F59E0B?style=for-the-badge" />
  </p>

  <p>
    <a href="#features">
      <img alt="Features" src="https://img.shields.io/badge/Features-00C2A8?style=for-the-badge" />
    </a>
    <a href="#architecture">
      <img alt="Architecture" src="https://img.shields.io/badge/Architecture-7C3AED?style=for-the-badge" />
    </a>
    <a href="#how-it-works">
      <img alt="How it works" src="https://img.shields.io/badge/How_It_Works-2563EB?style=for-the-badge" />
    </a>
    <a href="#why-i-built-this">
      <img alt="Why I built this" src="https://img.shields.io/badge/Why_I_Built_This-FF3864?style=for-the-badge" />
    </a>
  </p>

</div>

---

## Overview

**Brainfuck Calculator** is a tiny terminal calculator implemented in Brainfuck, an esoteric language with only eight commands:

```text
> < + - . , [ ]
```

That means there are no variables, functions, strings, structs, arrays in the normal sense, or numeric parsers. Everything is built out of a memory tape, a pointer, cell increments/decrements, input/output bytes, and loops.

The result is a deliberately difficult calculator that supports three arithmetic paths:

## Features

<table>
  <tr>
    <td><b>Brainfuck-only source</b></td>
    <td>The calculator logic lives in <code>main.bf</code>, not a higher-level wrapper.</td>
  </tr>
  <tr>
    <td><b>Terminal menu</b></td>
    <td>Prints a simple operation menu and routes input into the selected operation branch.</td>
  </tr>
  <tr>
    <td><b>ASCII input handling</b></td>
    <td>Reads raw characters from stdin and converts digit characters by subtracting ASCII offsets.</td>
  </tr>
  <tr>
    <td><b>Single-digit arithmetic</b></td>
    <td>Designed around one-digit operands, which keeps the tape logic survivable.</td>
  </tr>
  <tr>
    <td><b>Branching through cells</b></td>
    <td>Uses cell flags and loops to select addition, subtraction, or multiplication.</td>
  </tr>
  <tr>
    <td><b>Compiled executable included</b></td>
    <td>The repo includes <code>a.out</code>, a Linux executable for the current build.</td>
  </tr>
</table>

## Tech Stack

<p>
  <img alt="Brainfuck" src="https://img.shields.io/badge/Brainfuck-111827?style=flat-square" />
  <img alt="CLI" src="https://img.shields.io/badge/CLI-0F766E?style=flat-square" />
  <img alt="ASCII" src="https://img.shields.io/badge/ASCII_IO-F97316?style=flat-square" />
  <img alt="Tape Memory" src="https://img.shields.io/badge/Tape_Memory-9333EA?style=flat-square" />
  <img alt="Linux Binary" src="https://img.shields.io/badge/Linux_Binary-1D4ED8?style=flat-square&logo=linux&logoColor=white" />
</p>

| Layer | Technology | Role |
| --- | --- | --- |
| Language | `Brainfuck` | Entire calculator program and arithmetic logic. |
| Interface | Terminal stdin/stdout | Menu, operation selection, operand input, and result output. |
| Runtime model | Memory tape | Stores flags, operands, temporary values, and output cells. |
| Distribution | `a.out` | Included compiled Linux executable. |

## Project Structure

```text
.
|-- main.bf    # 333-line Brainfuck calculator source
|-- a.out      # Existing compiled Linux executable
`-- README.md  # Project documentation
```

## Architecture

```text
main.bf
|-- menu printer
|   `-- emits the operation list one character at a time
|
|-- operation selector
|   |-- reads menu input with ,
|   |-- compares against ASCII choices
|   `-- activates one operation branch
|
|-- addition branch
|   |-- reads two digit characters
|   |-- subtracts ASCII '0'
|   `-- combines values on the tape
|
|-- subtraction branch
|   |-- reads two digit characters
|   |-- converts from ASCII
|   `-- subtracts one cell value from another
|
`-- multiplication branch
    |-- reads two digit characters
    |-- converts from ASCII
    |-- performs repeated addition through loops
    `-- prints a cleaner decimal-style result
```

The architecture is not organized with functions because Brainfuck does not have functions. Instead, the program is structured through tape zones: groups of cells act as flags, input storage, temporary counters, arithmetic buffers, and output cells.

## Run

If you are on Linux, run the included executable:

```bash
./a.out
```

The program prints:

```text
1)ADDING
2)SUBTRACT
3)MULTIPLY
```

Choose an operation by typing `1`, `2`, or `3`, then provide the operands as raw input characters.

Example multiplication input:

```bash
printf '345' | ./a.out
```

This selects operation `3`, then multiplies `4` by `5`.

Expected ending output:

```text
20
```

## Input Notes

Brainfuck reads bytes, not friendly typed values. This program expects simple raw input:

| Input rule | Reason |
| --- | --- |
| Use single-digit operands | Multi-digit parsing would require a much larger tape protocol. |
| Use menu choices `1`, `2`, `3` | Operation routing is built around ASCII character comparison. |
| Expect rough output in some paths | Addition and subtraction still use character-oriented output behavior. |
| Multiplication is the cleanest path | It is the most polished operation in the current build. |

## How It Works

### 1. Printing Text

Every menu character is created by incrementing cells until they contain the right ASCII value, then printing with `.`.

There are no strings. A phrase like `ADDING` is built one byte at a time.

### 2. Reading Input

Input is read with:

```bf
,
```

The program receives ASCII characters, so digit input like `4` starts as ASCII code `52`, not numeric value `4`.

### 3. ASCII Conversion

To turn a digit character into a number, the program subtracts the ASCII offset for `0`:

```text
'4' -> 52
52 - 48 -> 4
```

In Brainfuck, that becomes repeated `-` operations and careful pointer positioning.

### 4. Arithmetic

Arithmetic is done by moving values between cells:

| Operation | Brainfuck strategy |
| --- | --- |
| Addition | Move counts from one cell into another. |
| Subtraction | Decrement one value while consuming another. |
| Multiplication | Repeated addition using counters and temporary cells. |

### 5. Output

Output is printed with:

```bf
.
```

For clean numeric output, the program must convert computed values back into printable ASCII. That is why multiplication is the most polished path: it has the most complete result-printing logic.

## Why I Built This

> yea i have no reason for building this one repo , i m just a masochist atp i just saw a reel saying " brainfuck is the worst programming language ever!" and yea, here i am

## Known Limitations

| Limitation | Current state |
| --- | --- |
| Input parsing | Raw character input only. |
| Operand size | Single-digit numbers are expected. |
| Output formatting | Addition/subtraction may display ASCII-style output instead of clean decimal text. |
| Maintainability | Brainfuck makes even small changes risky. |
| Portability | Included `a.out` is a Linux binary. |

## Roadmap Ideas

| Idea | Why it would help |
| --- | --- |
| Commented tape map | Document which cells hold flags, operands, counters, and output values. |
| Cleaner decimal output | Make addition and subtraction print like multiplication. |
| Interpreter instructions | Add commands for running `main.bf` with common Brainfuck interpreters. |
| Multi-digit support | Turn it from a tiny calculator into a real parser challenge. |
| Rebuild script | Document how `a.out` was produced. |

---

<div align="center">
  <sub>Built with eight commands, one memory tape, and an unreasonable amount of pointer movement.</sub>
</div>
