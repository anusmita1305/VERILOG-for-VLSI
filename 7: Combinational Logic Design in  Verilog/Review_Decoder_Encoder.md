# Verilog Review Questions – Decoders and Encoders

## 11. What is the Purpose of a Decoder in Digital Circuits?

A **decoder** is a combinational circuit that converts an `n-bit` binary input into **one of `2^n` output lines**.

### Example: 3-to-8 Decoder

* Inputs = 3
* Outputs = 8
* Only **one output is active** for each input combination.

```text
3-bit Input → Decoder → 8 Outputs
```

### Applications

* Memory address decoding
* Instruction decoding
* Data selection
* Control logic

**Interview Point:**
A decoder converts a **binary code into one active output line**.

---

## 12. Implement a 3-to-8 Decoder Using Behavioral Modeling

```verilog
module decoder_3to8 (
    input [2:0] A,
    output reg [7:0] Y
);

always @(*) begin
    case (A)
        3'b000: Y = 8'b00000001;
        3'b001: Y = 8'b00000010;
        3'b010: Y = 8'b00000100;
        3'b011: Y = 8'b00001000;
        3'b100: Y = 8'b00010000;
        3'b101: Y = 8'b00100000;
        3'b110: Y = 8'b01000000;
        3'b111: Y = 8'b10000000;
        default: Y = 8'b00000000;
    endcase
end

endmodule
```

**Interview Point:**
Behavioral combinational logic is commonly implemented using `always @(*)` with `case`.

---

## 13. Binary Encoder vs Priority Encoder

| Feature               | Binary Encoder                       | Priority Encoder                                      |
| --------------------- | ------------------------------------ | ----------------------------------------------------- |
| Function              | Converts active input to binary code | Converts highest-priority active input to binary code |
| Multiple inputs = `1` | Ambiguous/undefined                  | Handles multiple `1`s                                 |
| Priority              | No priority                          | Has defined priority                                  |
| Typical use           | Simple encoding                      | Interrupt/control logic                               |

### Example

For a 4-to-2 encoder:

```text
Input:  0001 → Output: 00
Input:  0010 → Output: 01
Input:  0100 → Output: 10
Input:  1000 → Output: 11
```

**Interview Point:**
A priority encoder can produce a valid output even when **multiple inputs are active**.

---

## 14. Verilog Code for a 4-to-2 Priority Encoder

Assume `I3` has the **highest priority**.

```verilog
module priority_encoder_4to2 (
    input [3:0] I,
    output reg [1:0] Y
);

always @(*) begin
    if (I[3])
        Y = 2'b11;
    else if (I[2])
        Y = 2'b10;
    else if (I[1])
        Y = 2'b01;
    else if (I[0])
        Y = 2'b00;
    else
        Y = 2'b00;
end

endmodule
```

### Priority Order

```text
I3 > I2 > I1 > I0
```

For example:

```text
I = 4'b1010

I3 = 1 → Output = 11
```

`I1` is ignored because `I3` has higher priority.

**Interview Point:**
The `if-else if` structure naturally implements **priority logic**.

---

## 15. What Happens if Multiple Inputs are `1` in a Binary Encoder?

A basic binary encoder assumes that **only one input is active at a time**.

If multiple inputs are `1`, the output becomes **ambiguous or undefined**.

### Example

For a 4-to-2 encoder:

```text
I = 4'b0110
```

Both `I2` and `I1` are `1`.

The encoder cannot determine which input should be encoded.

### How Does a Priority Encoder Solve This?

A priority encoder assigns a **priority order** to the inputs.

For example:

```text
I3 > I2 > I1 > I0
```

If multiple inputs are `1`, the encoder selects the **highest-priority active input**.

**Interview Point:**
A priority encoder removes the ambiguity of multiple active inputs by assigning **priority to each input**.

---

# Quick Revision

```text
Decoder
→ n inputs → 2^n outputs
→ One output active at a time

Encoder
→ 2^n inputs → n-bit output
→ Assumes one active input

Priority Encoder
→ Handles multiple active inputs
→ Selects highest-priority input

Decoder → Binary code → One active output
Encoder → One active input → Binary code

Priority Encoder → Multiple active inputs → Highest-priority input
```

### Key Interview Points

* **Decoder:** `n → 2^n`
* **Encoder:** `2^n → n`
* **Priority encoder:** Handles multiple active inputs.
* `case` → Commonly used for decoder implementation.
* `if-else if` → Naturally models priority logic.
* Always define priority clearly, e.g. `I3 > I2 > I1 > I0`.

