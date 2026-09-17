# Verilog Review Questions – Module, Testbench and Sequential Logic

## 1. What is the Structure of a Verilog Module?

A Verilog module generally contains:

1. Module declaration
2. Input/output ports
3. Internal signals
4. Logic implementation
5. `endmodule`

### Example

```verilog
module and_gate (
    input A,
    input B,
    output Y
);

assign Y = A & B;

endmodule
```

**Interview Point:**
A module is the basic building block used to describe a hardware component in Verilog.

---

## 2. What are the Differences Between Simulation and Synthesis?

| Simulation                        | Synthesis                         |
| --------------------------------- | --------------------------------- |
| Verifies design behavior          | Converts RTL into hardware        |
| Uses a simulator                  | Uses a synthesis tool             |
| Produces waveforms/output         | Produces gate-level netlist       |
| Does not create physical hardware | Represents implementable hardware |

**Interview Point:**
**Simulation → Verify functionality**
**Synthesis → Convert RTL into hardware logic**

---

## 3. How Do `assign` Statements Work in Verilog?

`assign` creates a **continuous assignment**.

```verilog
assign Y = A & B;
```

Whenever `A` or `B` changes, `Y` is automatically updated.

**Interview Point:**
`assign` is commonly used to describe **combinational logic** and drives a net such as `wire`.

---

## 4. Verilog Code for a 2-Input OR Gate

```verilog
module or_gate (
    input A,
    input B,
    output Y
);

assign Y = A | B;

endmodule
```

---

## 5. How Does an `always` Block Differ from `assign`?

| `assign`                                     | `always`                                       |
| -------------------------------------------- | ---------------------------------------------- |
| Continuous assignment                        | Procedural block                               |
| Commonly used for simple combinational logic | Used for combinational and sequential logic    |
| Typically drives `wire`                      | Outputs are commonly declared `reg` in Verilog |
| No sensitivity list                          | Uses event control/sensitivity list            |

### Example

```verilog
assign Y = A & B;
```

```verilog
always @(*) begin
    Y = A & B;
end
```

**Interview Point:**
`assign` describes continuous dataflow, while `always` describes procedural behavior.

---

## 6. Explain the Purpose of Testbenches in Verilog

A **testbench** is a verification environment used to test the **DUT (Design Under Test)**.

It:

* Generates input stimulus.
* Applies different test cases.
* Observes outputs.
* Helps detect design errors.

**Interview Point:**
A testbench verifies whether the DUT behaves as expected.

---

## 7. What is the Role of `$monitor` in a Testbench?

`$monitor` continuously displays signal values whenever one of its arguments changes.

```verilog
$monitor("Time=%0t A=%b B=%b Y=%b", $time, A, B, Y);
```

**Interview Point:**
`$monitor` is useful for observing signal changes during simulation.

---

## 8. How Does a MUX Function in Verilog?

A **MUX selects one input from multiple inputs based on select lines**.

### 2:1 MUX

```verilog
assign Y = S ? I1 : I0;
```

```text
S = 0 → Y = I0
S = 1 → Y = I1
```

**Interview Point:**
A MUX performs **data selection**.

---

## 9. What Does `posedge` Signify?

`posedge` means **positive/rising edge** of a signal.

```verilog
always @(posedge clk)
```

This block executes when `clk` changes:

```text
0 → 1
```

**Interview Point:**
`posedge clk` is commonly used to model **positive-edge-triggered sequential circuits** such as flip-flops.

---

## 10. Testbench for a 4-Input AND Gate

### DUT

```verilog
module and_4input (
    input A,
    input B,
    input C,
    input D,
    output Y
);

assign Y = A & B & C & D;

endmodule
```

### Testbench

```verilog
module tb_and_4input;

reg A, B, C, D;
wire Y;

and_4input DUT (
    .A(A),
    .B(B),
    .C(C),
    .D(D),
    .Y(Y)
);

initial begin
    A=0; B=0; C=0; D=0; #10;
    A=1; B=1; C=1; D=1; #10;
    A=1; B=1; C=1; D=0; #10;
    A=1; B=0; C=1; D=1; #10;

    $finish;
end

initial begin
    $monitor("Time=%0t A=%b B=%b C=%b D=%b Y=%b",
             $time, A, B, C, D, Y);
end

endmodule
```

**Interview Point:**
The testbench should test both **valid output conditions and different input combinations**.

---

## 11. Difference Between `reg` and `wire`

| `wire`                          | `reg`                                       |
| ------------------------------- | ------------------------------------------- |
| Represents a net                | Represents a procedural variable            |
| Commonly driven by `assign`     | Commonly assigned inside `always`/`initial` |
| Cannot be assigned procedurally | Can hold a value assigned procedurally      |
| Common for module connections   | Common for procedural outputs/signals       |

### Example

```verilog
wire Y;
assign Y = A & B;
```

```verilog
reg Y;

always @(*) begin
    Y = A & B;
end
```

**Important:**
`reg` does **not necessarily mean a physical hardware register**. Hardware type depends on the logic being described.

---

## 12. Implement a 4:1 MUX Using Verilog

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
A 4:1 MUX requires **2 select lines**.

---

## 13. What Happens When You Use `assign` Inside an `always` Block?

This is **not valid Verilog syntax**.

`assign` is a continuous assignment and is written outside procedural blocks.

### Correct

```verilog
assign Y = A & B;
```

Or:

```verilog
always @(*) begin
    Y = A & B;
end
```

**Interview Point:**
Do not use a continuous `assign` statement inside an `always` block.

---

## 14. Explain Blocking vs Non-Blocking Assignments

### Blocking (`=`)

* Executes immediately.
* Commonly used for combinational logic.

```verilog
always @(*) begin
    A = B;
    C = A;
end
```

### Non-Blocking (`<=`)

* Updates are scheduled for the end of the current simulation time step.
* Commonly used for sequential logic.

```verilog
always @(posedge clk) begin
    Q <= D;
end
```

| Blocking `=`                    | Non-Blocking `<=`                        |
| ------------------------------- | ---------------------------------------- |
| Immediate assignment            | Scheduled assignment                     |
| Commonly combinational          | Commonly sequential                      |
| Statement execution is blocking | Statements can be evaluated concurrently |

**Interview Point:**
**Combinational → `=`**
**Sequential → `<=`**

---

## 15. What is the Significance of `#10` Delay in Testbenches?

`#10` introduces a **simulation delay of 10 time units**.

Example:

```verilog
A = 1;
#10;
B = 1;
```

Here, `B` is assigned 10 simulation time units after `A`.

**Interview Point:**
`#10` is commonly used in **testbenches to create timing gaps between stimulus**.

> The actual duration depends on the `` `timescale `` or SystemVerilog time-unit settings.

---

## 16. How Do You Generate a Clock Signal in Verilog?

A common testbench method is:

```verilog
reg clk;

initial begin
    clk = 0;
    forever #5 clk = ~clk;
end
```

This creates a clock with:

```text
Half-period = 5 time units
Period      = 10 time units
```

**Interview Point:**
A clock can be generated by repeatedly **toggling the clock signal** using a `forever` loop.

---

## 17. Verilog Code for a JK Flip-Flop

```verilog
module jk_ff (
    input J,
    input K,
    input clk,
    output reg Q
);

always @(posedge clk) begin
    case ({J, K})
        2'b00: Q <= Q;     // No change
        2'b01: Q <= 1'b0;  // Reset
        2'b10: Q <= 1'b1;  // Set
        2'b11: Q <= ~Q;    // Toggle
    endcase
end

endmodule
```

### JK Flip-Flop Operation

| J | K | Q(next)   |
| - | - | --------- |
| 0 | 0 | No change |
| 0 | 1 | 0         |
| 1 | 0 | 1         |
| 1 | 1 | Toggle    |

**Interview Point:**
For `J=K=1`, the JK flip-flop **toggles its output**.

---

## 18. Common Mistakes in Synthesizable Verilog

Common mistakes include:

* Incomplete assignments in combinational `always` blocks → may infer **latches**.
* Using incorrect sensitivity lists.
* Using `=` instead of `<=` for sequential logic.
* Multiple drivers for the same signal.
* Using unsupported/non-synthesizable constructs in RTL.
* Forgetting reset conditions where required.
* Creating unintended combinational feedback.
* Mixing combinational and sequential logic incorrectly.

### Example of an Incomplete Assignment

```verilog
always @(*) begin
    if (en)
        Y = A;
end
```

When `en=0`, `Y` retains its previous value, potentially inferring a **latch**.

**Interview Point:**
For combinational logic, ensure that outputs are assigned for **all possible conditions**.

---

## 19. How Does the `initial` Block Work in Verilog?

An `initial` block starts execution at **simulation time 0** and executes **only once**.

### Example

```verilog
initial begin
    A = 0;
    B = 0;
    #10 A = 1;
end
```

`initial` blocks are commonly used in **testbenches** for:

* Generating stimulus
* Initializing signals
* Controlling simulation

**Interview Point:**
`initial` is mainly used for **simulation/testbench purposes**. It is not generally used for ASIC synthesizable RTL.

---

## 20. Implement a 2-Bit Binary Counter

```verilog
module counter_2bit (
    input clk,
    input reset,
    output reg [1:0] count
);

always @(posedge clk or posedge reset) begin
    if (reset)
        count <= 2'b00;
    else
        count <= count + 1'b1;
end

endmodule
```

### Counting Sequence

```text
00 → 01 → 10 → 11 → 00 → ...
```

**Interview Point:**
A 2-bit counter has **4 states** (`0` to `3`) and automatically wraps back to `00`.

---

# Quick Revision

```text
Module
→ Basic building block of Verilog

Simulation
→ Verifies behavior

Synthesis
→ Converts RTL into hardware/netlist

assign
→ Continuous assignment

always
→ Procedural block

wire
→ Net, commonly driven by assign

reg
→ Procedural variable in Verilog

posedge
→ Rising edge: 0 → 1

$monitor
→ Displays signal changes

#10
→ 10 simulation time-unit delay

initial
→ Executes once

forever
→ Repeats continuously

Blocking (=)
→ Commonly used for combinational logic

Non-blocking (<=)
→ Commonly used for sequential logic

MUX
→ Selects one input from multiple inputs

JK FF
→ 00: Hold
→ 01: Reset
→ 10: Set
→ 11: Toggle

2-bit Counter
→ 00 → 01 → 10 → 11 → 00
```

### Key Interview Rules

```text
Combinational logic:
→ always @(*)
→ Blocking assignment (=)

Sequential logic:
→ always @(posedge clk)
→ Non-blocking assignment (<=)

Continuous combinational logic:
→ assign
```

