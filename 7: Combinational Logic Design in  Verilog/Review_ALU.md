# Verilog Review Questions – ALU

## 16. What is an ALU and What Operations Can It Perform?

An **ALU (Arithmetic Logic Unit)** is a digital circuit that performs **arithmetic and logical operations** on binary data.

### Common Operations

**Arithmetic:**

* Addition
* Subtraction

**Logical:**

* AND
* OR
* XOR
* NOT

An ALU is a fundamental component of a **CPU/processor datapath**.

**Interview Point:**
ALU performs arithmetic and logical operations based on **control/select signals**.

---

## 17. Implement a 4-bit ALU

The following ALU supports:

| `sel` | Operation   |
| ----- | ----------- |
| `000` | Addition    |
| `001` | Subtraction |
| `010` | AND         |
| `011` | OR          |
| `100` | XOR         |

```verilog
module alu_4bit (
    input [3:0] A,
    input [3:0] B,
    input [2:0] sel,
    output reg [3:0] Y
);

always @(*) begin
    case (sel)
        3'b000: Y = A + B;
        3'b001: Y = A - B;
        3'b010: Y = A & B;
        3'b011: Y = A | B;
        3'b100: Y = A ^ B;
        default: Y = 4'b0000;
    endcase
end

endmodule
```

**Interview Point:**
The `sel` signal determines which operation the ALU performs.

---

## 18. How is `case` Used to Implement an ALU Operation Selector?

The `case` statement maps each **control/select value** to a particular ALU operation.

Example:

```verilog
case (sel)
    3'b000: Y = A + B;
    3'b001: Y = A - B;
    3'b010: Y = A & B;
    3'b011: Y = A | B;
    3'b100: Y = A ^ B;
    default: Y = 4'b0000;
endcase
```

Here:

```text
sel = 000 → Addition
sel = 001 → Subtraction
sel = 010 → AND
sel = 011 → OR
sel = 100 → XOR
```

**Interview Point:**
`case` is useful for implementing **multiple mutually exclusive operations controlled by select signals**.

---

## 19. Testbench for a 4-bit ALU

```verilog
module tb_alu_4bit;

reg [3:0] A;
reg [3:0] B;
reg [2:0] sel;
wire [3:0] Y;

alu_4bit DUT (
    .A(A),
    .B(B),
    .sel(sel),
    .Y(Y)
);

initial begin

    // Addition
    A = 4'b0101;
    B = 4'b0011;
    sel = 3'b000;
    #10;

    // Subtraction
    A = 4'b0101;
    B = 4'b0011;
    sel = 3'b001;
    #10;

    // AND
    A = 4'b1010;
    B = 4'b1100;
    sel = 3'b010;
    #10;

    // OR
    A = 4'b1010;
    B = 4'b1100;
    sel = 3'b011;
    #10;

    // XOR
    A = 4'b1010;
    B = 4'b1100;
    sel = 3'b100;
    #10;

    $finish;
end

initial begin
    $monitor("Time=%0t A=%b B=%b sel=%b Y=%b",
              $time, A, B, sel, Y);
end

endmodule
```

### Expected Results

| Operation   | A      | B      | Expected Y |
| ----------- | ------ | ------ | ---------- |
| Addition    | `0101` | `0011` | `1000`     |
| Subtraction | `0101` | `0011` | `0010`     |
| AND         | `1010` | `1100` | `1000`     |
| OR          | `1010` | `1100` | `1110`     |
| XOR         | `1010` | `1100` | `0110`     |

**Interview Point:**
A testbench should apply different input combinations and verify the output for **every supported operation**.

---

## 20. What is a Parameterized ALU? What are its Advantages?

A **parameterized ALU** allows the data width to be changed without rewriting the module.

### Example

```verilog
module alu #(parameter WIDTH = 4) (
    input [WIDTH-1:0] A,
    input [WIDTH-1:0] B,
    input [2:0] sel,
    output reg [WIDTH-1:0] Y
);

always @(*) begin
    case (sel)
        3'b000: Y = A + B;
        3'b001: Y = A - B;
        3'b010: Y = A & B;
        3'b011: Y = A | B;
        3'b100: Y = A ^ B;
        default: Y = {WIDTH{1'b0}};
    endcase
end

endmodule
```

The width can then be changed during instantiation:

```verilog
alu #(8)  ALU8  (...);
alu #(16) ALU16 (...);
alu #(32) ALU32 (...);
```

### Advantages

* **Reusable** for different data widths.
* Reduces code duplication.
* Easier to modify and maintain.
* Supports scalable hardware design.

**Interview Point:**
Parameters make a Verilog module **configurable and reusable** without changing the actual module code.

---

# Quick Revision

```text
ALU
→ Arithmetic + Logic operations
→ Controlled using select/control signals

case
→ Selects the required ALU operation

Testbench
→ Applies inputs → Checks ALU outputs

Parameterized ALU
→ Same ALU design → Different data widths

parameter WIDTH = 4
→ Makes ALU width configurable
```

### Key Interview Points

* ALU = **Arithmetic Logic Unit**
* `sel` determines the operation.
* `case` is commonly used for an ALU operation selector.
* Testbench should verify **all operations**.
* Parameterization improves **reusability and scalability**.
