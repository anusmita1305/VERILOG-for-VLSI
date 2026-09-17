# Verilog Review Questions – Assignments, FPGA and ASIC

## 19. Differentiate Between Blocking and Non-Blocking Assignments

Verilog has two main types of procedural assignments:

### Blocking Assignment (`=`)

* Executes **immediately**.
* The next statement executes only after the current assignment is completed.
* Commonly used for **combinational logic**.

```verilog
always @(*) begin
    A = B;
    C = A;
end
```

Here, `C` receives the **new value of `A`**.

### Non-Blocking Assignment (`<=`)

* Updates the left-hand side **after the current simulation time step**.
* Multiple assignments can be evaluated without blocking each other.
* Commonly used for **sequential logic** such as flip-flops.

```verilog
always @(posedge clk) begin
    Q <= D;
end
```

### Quick Comparison

| Feature       | Blocking (`=`)         | Non-Blocking (`<=`)     |
| ------------- | ---------------------- | ----------------------- |
| Execution     | Immediate              | Scheduled update        |
| Common use    | Combinational logic    | Sequential logic        |
| Typical block | `always @(*)`          | `always @(posedge clk)` |
| Models        | Combinational behavior | Flip-flops/registers    |

**Interview Point:**

> **Blocking (`=`) → Combinational logic**
> **Non-blocking (`<=`) → Sequential logic**

---

## 20. What is the Significance of FPGA and ASIC in Modern Electronics?

### FPGA — Field Programmable Gate Array

An FPGA is a **programmable hardware device** that can be configured after manufacturing.

### Key Characteristics

* Reprogrammable
* Faster development/prototyping
* Suitable for low-volume applications
* Useful for testing hardware designs

**Applications:**

* Prototyping
* Signal processing
* Networking
* Aerospace and defense
* Hardware acceleration

---

### ASIC — Application-Specific Integrated Circuit

An ASIC is a chip **designed for a specific application or purpose**.

### Key Characteristics

* High performance
* Lower power can be achieved through optimization
* Smaller area compared with an equivalent FPGA implementation
* High initial design/manufacturing cost
* Not generally reprogrammable after fabrication

**Applications:**

* CPUs and GPUs
* Smartphone chips
* AI accelerators
* Networking chips
* Automotive electronics

---

### FPGA vs ASIC

| Feature          | FPGA                     | ASIC                    |
| ---------------- | ------------------------ | ----------------------- |
| Programmability  | Reprogrammable           | Fixed after fabrication |
| Initial cost     | Lower                    | Higher                  |
| Development time | Shorter                  | Longer                  |
| Performance      | Generally lower          | Generally higher        |
| Power efficiency | Generally lower          | Generally higher        |
| Best suited for  | Prototyping, flexibility | High-volume products    |

**Interview Point:**

> **FPGA → Flexible and reprogrammable**
> **ASIC → Application-specific and optimized for performance, power, and area**

---

# Quick Revision

```text
Blocking (=)
→ Immediate execution
→ Commonly used for combinational logic

Non-Blocking (<=)
→ Scheduled update
→ Commonly used for sequential logic

FPGA
→ Programmable
→ Reprogrammable
→ Good for prototyping and flexibility

ASIC
→ Application-specific
→ Fixed after fabrication
→ Optimized for PPA and high-volume products
```
re
