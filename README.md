# ⚡ 4-Bit Arithmetic Logic Unit (ALU)

> A 4-bit ALU designed and implemented both on hardware (using IC gates on a trainer board) and in software (Verilog HDL simulated in ModelSim), capable of performing 5 arithmetic and logical operations.

![Status](https://img.shields.io/badge/Status-Completed-brightgreen)
![Domain](https://img.shields.io/badge/Domain-Digital%20Logic%20Design-blue)
![Type](https://img.shields.io/badge/Type-University%20Project-orange)
![HDL](https://img.shields.io/badge/HDL-Verilog-purple)

---

## 📌 Overview

This semester project involves designing a **4-bit Arithmetic Logic Unit** that accepts two 4-bit binary inputs and performs one of five operations selected via 2-bit control lines. The ALU was first built physically on an **Analog & Digital Training System (MCP M21-7000)** using discrete logic gate ICs, a 4-bit full adder, and 4×1 multiplexers. The design was then verified in software using **Verilog HDL** and simulated in **ModelSim**. Output is demonstrated through 4 LEDs on the trainer board.

---

## 🎯 Objectives

- Design a 4-bit ALU circuit on hardware using standard logic ICs
- Implement the same ALU in Verilog HDL for simulation-based verification
- Support 5 operations: Addition, Subtraction, Bitwise AND, Bitwise OR, Bitwise NOT
- Demonstrate output through 4 LEDs on the trainer board

---

## ⚙️ ALU Operations

The operation is selected using a **2-bit select line (S1, S0)** and a **mode bit (M)**:

| S1 | S0 | M | Operation              | Description                     |
|----|----|---|------------------------|---------------------------------|
| 0  | 0  | 0 | **Addition**           | A + B with carry propagation    |
| 0  | 0  | 1 | **Subtraction**        | A - B using borrow propagation  |
| 0  | 1  | — | **Bitwise AND**        | A & B                           |
| 1  | 0  | — | **Bitwise OR**         | A \| B                          |
| 1  | 1  | — | **Bitwise NOT**        | ~A (complement of A)            |

---

## 🏗️ Architecture

The ALU is built around 5 core classes of logic, routed through **4×1 multiplexers (IC 74153)** that select which operation's result reaches the output LEDs:

```
          ┌─────────┐
  A[3:0] ──┤         ├── ADD/SUB result ──┐
  B[3:0] ──┤ 4-Bit   │                    │
  Cin    ──┤ Adder   │                    │
           └─────────┘                    │
                                          ▼
  A[3:0] ──┬── AND Gates ──────────┐  ┌──────────┐
           ├── OR  Gates ──────────┼─▶│  4×1 MUX │──▶ Output[3:0]
           ├── NOT Gates ──────────┘  │ (IC74153)│      (LEDs)
           │                          └──────────┘
           │                              ▲
  S1, S0 ─────────────────────────────────┘
```

---

## 🧰 Tech Stack

### Hardware Components

| Component            | IC Number | Purpose                          |
|---------------------|-----------|----------------------------------|
| OR Gate             | 7432      | Bitwise OR operation             |
| AND Gate            | 7408      | Bitwise AND operation            |
| NOT Gate (Inverter) | 7404      | Bitwise NOT operation            |
| XOR Gate            | 7486      | Used in adder/subtractor circuit |
| 4-Bit Full Adder    | —         | Addition & subtraction           |
| 4×1 Multiplexer     | 74153     | Operation selection via S1, S0   |

### Other Hardware
- MCP M21-7000 Analog & Digital Training System
- Breadboard, jumper wires, LEDs

### Software

| Tool      | Purpose                                   |
|-----------|-------------------------------------------|
| Verilog HDL | Hardware description & behavioral modeling |
| ModelSim  | Simulation & waveform verification         |

---

## 💻 Verilog Implementation

The ALU is implemented as a single Verilog module with behavioral modeling. The select lines control which operation is active, and the mode bit `m` toggles between addition and subtraction when `select == 00`.

```verilog
module project(out, select, a, b, cin, sum, cout, bor, diff, m);
    input m, cin;
    input [3:0] a, b;
    input [1:0] select;
    output reg [3:0] out, cout, bor, diff, sum;

    always @(a or b or cin) begin
        if (select == 2'b00) begin
            if (m == 0)      // Addition
                // 4-stage full adder with carry propagation
            else             // Subtraction
                // 4-stage subtractor with borrow propagation
        end
        else if (select == 2'b01) out = a & b;  // AND
        else if (select == 2'b10) out = a | b;  // OR
        else if (select == 2'b11) out = ~a;     // NOT
    end
endmodule
```

> Full source code is available in the [`src/`](./src/) directory.

---

## 🔬 Design Methodology

The project uses three key digital design relationships:

| Concept         | Application in Project                                      |
|----------------|-------------------------------------------------------------|
| **Composition** | MUX contains and routes outputs from individual gate arrays |
| **Association** | Adder/subtractor shares input buses with logic gates        |
| **Selection**   | 2-bit select lines + mode bit choose among 5 operations     |

---

## 📁 Repository Structure

```
├── README.md                    # Project documentation
├── Report.pdf                   # Full project report (13 pages)
├── src/
│   └── alu_4bit.v               # Verilog source code
└── docs/
    └── logic_diagram.png        # Hand-drawn logic diagram
```

---

## 📄 Documentation

| Document             | Link                            |
|---------------------|----------------------------------|
| 📘 Full Report (PDF) | [View Report](./Report.pdf)      |

---

## 📸 Screenshots

<details>
<summary>Click to view hardware & diagrams</summary>

### Hardware Implementation
The ALU was built on an MCP M21-7000 trainer board with labeled IC chips (NOT, XOR, OR, AND, MUX) connected via jumper wires. Inputs are controlled via data switches (A0-A3, B0-B3, M, En, S1, S0) and outputs are displayed on BCD displays/LEDs.

### Logic Diagram
Hand-drawn schematic showing two 4-bit input buses routed through AND, OR, NOT, and XOR gate arrays, with outputs fed into four 4×1 MUXes to produce the final 4-bit result.

</details>

---

## 👥 Team

| Name             | Roll Number       |
|-----------------|-------------------|
| Ahtisham Saleem  | NIM-BSEE-2021-19 |
| M. Rafique       | NIM-BSEE-2021-41 |
| M. Makki         | NIM-BSEE-2021-23 |

---

## 🏫 Academic Info

| Detail          | Info                              |
|----------------|-----------------------------------|
| Course          | Digital Logic Design (DLD)        |
| Supervisor      | Mam Naureen Shaukat               |
| Program         | BS Electrical Engineering         |
| Semester        | 2022                              |

---

## 📬 Contact

- **Email:** engr.ahtishamsaleem@gmail.com
- **LinkedIn:** [Ahtisham Saleem](https://www.linkedin.com/in/ahtisham-salim)
- **GitHub:** [@codebyahtisham](https://github.com/codebyahtisham)

---

<p align="center">
  ⭐ If you found this project interesting, consider giving it a star!
</p>
