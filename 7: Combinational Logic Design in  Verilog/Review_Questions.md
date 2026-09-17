# Verilog Review Questions – Combinational Logic

## 1. Difference between Bitwise AND (`&`) and Logical AND (`&&`)

| Feature    | Bitwise AND `&`                  | Logical AND `&&`                      |
| ---------- | -------------------------------- | ------------------------------------- |
| Operation  | Operates bit-by-bit              | Treats operands as logical conditions |
| Result     | Can be multi-bit                 | Always 1-bit (`0` or `1`)             |
| Common use | Bit manipulation, hardware logic | Conditions in `if`, `while`, etc.     |

### Example

```verilog
4'b1010 & 4'b1100    // 4'b1000

4'b1010 && 4'b1100   // 1
```

**Interview Point:**
`&` performs **bitwise AND**, while `&&` performs **logical AND**.

---

## 2. Verilog Module for a 3-Input AND Gate

```verilog
module and_3input (
    input A,
    input B,
    input C,
    output Y
);

assign Y = A & B & C;

endmodule
```

### Logic

```text
Y = A · B · C
```

**Interview Point:**
The output is `1` only when **all three inputs are 1**.

---

## 3. NOR Gate Function and Verilog Implementation

A **NOR gate** is an OR gate followed by a NOT operation.

### Boolean Expression

```text
Y = ~(A | B)
```

### Verilog Implementation

```verilog
module nor_gate (
    input A,
    input B,
    output Y
);

assign Y = ~(A | B);

endmodule
```

### Truth Table

| A | B | Y |
| - | - | - |
| 0 | 0 | 1 |
| 0 | 1 | 0 |
| 1 | 0 | 0 |
| 1 | 1 | 0 |

**Interview Point:**
NOR output is `1` **only when all inputs are 0**.

---

## 4. XNOR Gate – Dataflow and Behavioral Modeling

XNOR output is `1` when both inputs are the **same**.

### A. Dataflow Modeling

```verilog
module xnor_dataflow (
    input A,
    input B,
    output Y
);

assign Y = ~(A ^ B);

endmodule
```

### B. Behavioral Modeling

```verilog
module xnor_behavioral (
    input A,
    input B,
    output reg Y
);

always @(*) begin
    Y = ~(A ^ B);
end

endmodule
```

**Interview Point:**
XNOR can be implemented as **NOT of XOR**:

```text
XNOR = ~(A ^ B)
```

---

## 5. How Does `assign` Help in Implementing Combinational Logic?

The `assign` statement is used for **continuous assignment**.

### Example

```verilog
wire Y;

assign Y = A & B;
```

Whenever `A` or `B` changes, `Y` is automatically updated.

### Key Points

* Used mainly for **combinational logic**.
* Represents **continuous hardware behavior**.
* Does not require an `always` block.
* Typically used to drive a `wire`.

**Interview Point:**
`assign` continuously evaluates the expression and updates the output whenever an input changes.

---

# Quick Revision

| Operator / Construct | Meaning                           |            |            |
| -------------------- | --------------------------------- | ---------- | ---------- |
| `&`                  | Bitwise AND                       |            |            |
| `&&`                 | Logical AND                       |            |            |
| `                    | `                                 | Bitwise OR |            |
| `                    |                                   | `          | Logical OR |
| `^`                  | XOR                               |            |            |
| `~^` or `^~`         | XNOR                              |            |            |
| `~`                  | NOT                               |            |            |
| `assign`             | Continuous assignment             |            |            |
| `always @(*)`        | Behavioral combinational modeling |            |            |

### Key Interview Takeaways

* `&` → **Bitwise operation**
* `&&` → **Logical operation**
* `assign` → **Continuous assignment**
* `always @(*)` → **Combinational behavioral modeling**
* XNOR → Output is `1` when inputs are **equal**
* NOR → Output is `1` when **all inputs are 0**
