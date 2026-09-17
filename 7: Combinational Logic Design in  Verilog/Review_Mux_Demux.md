# Verilog Review Questions – Multiplexers and Demultiplexers

## 6. What is a Multiplexer (MUX), and Where is it Used?

A **Multiplexer (MUX)** is a combinational circuit that selects **one input from multiple inputs** and sends it to a single output.

For a MUX with `2^n` inputs, `n` select lines are required.

### Example: 4-to-1 MUX

* Inputs: `I0, I1, I2, I3`
* Select lines: `S1, S0`
* Output: `Y`

```text
S1 S0
00 → I0
01 → I1
10 → I2
11 → I3
```

### Applications

* Data selection and routing
* Bus selection
* ALU and processor datapaths
* Communication systems

**Interview Point:**
A MUX is essentially a **digital data selector**.

---

## 7. Verilog Code for a 4-to-1 MUX Using `case`

```verilog
module mux_4to1 (
    input I0,
    input I1,
    input I2,
    input I3,
    input [1:0] S,
    output reg Y
);

always @(*) begin
    case (S)
        2'b00: Y = I0;
        2'b01: Y = I1;
        2'b10: Y = I2;
        2'b11: Y = I3;
        default: Y = 1'b0;
    endcase
end

endmodule
```

**Interview Point:**
`case` is commonly used to describe **selection-based combinational logic** such as MUXes.

---

## 8. Difference Between DEMUX and MUX

| Feature      | MUX                      | DEMUX                    |
| ------------ | ------------------------ | ------------------------ |
| Full form    | Multiplexer              | Demultiplexer            |
| Function     | Many inputs → One output | One input → Many outputs |
| Main purpose | Data selection           | Data distribution        |
| Select lines | Select input             | Select output            |

### Simple Representation

```text
MUX:
Multiple Inputs → [ MUX ] → Single Output

DEMUX:
Single Input → [ DEMUX ] → Multiple Outputs
```

**Interview Point:**
A **MUX selects one input**, while a **DEMUX routes one input to one of multiple outputs**.

---

## 9. Implement an 8-to-1 MUX Using Two 4-to-1 MUXes

An 8-to-1 MUX can be constructed using:

* Two 4-to-1 MUXes for the first stage
* One 2-to-1 selection in the second stage

### Verilog Code

```verilog
module mux_8to1 (
    input [7:0] I,
    input [2:0] S,
    output Y
);

wire Y0, Y1;

mux_4to1 MUX0 (
    .I0(I[0]),
    .I1(I[1]),
    .I2(I[2]),
    .I3(I[3]),
    .S(S[1:0]),
    .Y(Y0)
);

mux_4to1 MUX1 (
    .I0(I[4]),
    .I1(I[5]),
    .I2(I[6]),
    .I3(I[7]),
    .S(S[1:0]),
    .Y(Y1)
);

assign Y = S[2] ? Y1 : Y0;

endmodule
```

### Structure

```text
I0-I3 ──> 4:1 MUX ──> Y0 ──┐
                            ├──> 2:1 MUX ──> Y
I4-I7 ──> 4:1 MUX ──> Y1 ──┘
                             ↑
                            S2

             S1,S0 → Both 4:1 MUXes
```

**Interview Point:**
`S[1:0]` selects an input within each 4:1 MUX, while `S[2]` selects between the two intermediate outputs.

> **Note:** The `mux_4to1` module from Question 7 must be available for this hierarchical implementation.

---

## 10. Advantage of the Ternary (`?:`) Operator for MUXes

The ternary operator provides a **compact way to describe MUX logic**.

### Example

```verilog
assign Y = S ? I1 : I0;
```

This represents a **2-to-1 MUX**:

```text
S = 0 → Y = I0
S = 1 → Y = I1
```

### Advantages

* Short and readable
* Directly represents MUX selection
* Useful for simple combinational logic
* Can be used with `assign`

**Interview Point:**
The ternary operator `?:` is a concise way to describe **conditional selection/MUX behavior** in Verilog.

---

# Quick Revision

```text
MUX   → Many inputs → One output
DEMUX → One input → Many outputs

4:1 MUX → 4 inputs, 2 select lines
8:1 MUX → 8 inputs, 3 select lines

case   → Useful for selection-based combinational logic
?:     → Compact way to describe MUX/conditional logic

MUX → Data selection
DEMUX → Data distribution
```

