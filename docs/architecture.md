# Architecture

## Overview

The Expression Interpreter is built around an Abstract Syntax Tree (AST).

Instead of evaluating expressions directly, the interpreter first converts user input into a tree representation and then recursively evaluates the tree.

Pipeline:

```text
Input
  |
  v
Tokenizer
  |
  v
Token Stream
  |
  v
AST Builder
  |
  v
Abstract Syntax Tree
  |
  v
Recursive Evaluation
```

---

## Tokenization

The tokenizer converts raw text into a sequence of tokens.

Example:

```text
max(1+2,3*4)
```

becomes:

```text
Variable(max)
(
Number(1)
+
Number(2)
,
Number(3)
*
Number(4)
)
```

The tokenizer also performs implicit multiplication insertion.

Example:

```text
2(3+4)
```

becomes:

```text
2 * (3+4)
```

---

## AST Construction

The interpreter uses two stacks:

* Operand Stack
* Operator Stack

The AST is constructed using a precedence-based reduction strategy similar to the Shunting Yard algorithm.

Example:

```text
8*3+1*2
```

AST:

```text
+
├── *
│   ├── 8
│   └── 3
└── *
    ├── 1
    └── 2
```

---

## Function Calls

Built-in functions are parsed as CallExpr nodes.

Example:

```text
max(1,2)
```

AST:

```text
Call(max)
├── 1
└── 2
```

Nested function calls are supported.

Example:

```text
max(max(1,2),min(3,4))
```

AST:

```text
Call(max)
├── Call(max)
│   ├── 1
│   └── 2
└── Call(min)
    ├── 3
    └── 4
```

---

## Evaluation

Evaluation is performed recursively.

Example:

```text
+
├── 1
└── *
    ├── 2
    └── 3
```

Evaluation order:

```text
2 * 3 = 6
1 + 6 = 7
```

Result:

```text
7
```

---

## Current Built-in Functions

```text
max(a,b)
min(a,b)
convertBool(x)
```

---

## Future Extensions

* Unary Operators
* Equality Operators
* Logical Operators
* Statement Parsing
* Semantic Analysis
* Compiler Frontend Integration

```
```
