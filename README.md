# RISC-V Processor Design using SystemVerilog

A SystemVerilog-based digital design project covering fundamental combinational and sequential circuits along with the implementation of a basic **32-bit RISC-V processor**.

The project follows a modular RTL design approach, starting from basic digital building blocks and progressing toward processor-level components such as instruction fetching, instruction decoding, arithmetic execution, branching, jumps, memory access, and register operations.

---

## Project Structure

```text
.
├── 1_combinational
│   ├── 4_input_adder
│   ├── hc_dec
│   ├── hc_enc
│   ├── prog_add_sub
│   ├── mux
│   ├── sorting_box
│   └── trailing_one_detect
│
├── 2_sequential
│   ├── dff
│   ├── division
│   ├── fifo
│   ├── fir_filter
│   ├── moving_average
│   ├── seq_detect
│   └── usb_decode
│
└── 3_riscv
    ├── alu.sv
    ├── alu_core.sv
    ├── branch.sv
    ├── decode_branch_inst.sv
    ├── decode_imm_inst.sv
    ├── decode_jump_inst.sv
    ├── decode_load_inst.sv
    ├── decode_reg_inst.sv
    ├── decode_store_inst.sv
    ├── decode_upperimm_inst.sv
    ├── idu.sv
    ├── ieu.sv
    ├── ifu.sv
    ├── inst_data_arbiter.sv
    ├── jump.sv
    ├── load.sv
    ├── mem.sv
    ├── processor.sv
    ├── processor_defines.sv
    ├── regfile.sv
    └── store.sv
```

---

## Project Overview

This project demonstrates the design of digital hardware using **SystemVerilog**, progressing from individual RTL components to a modular **32-bit RISC-V processor**.

The RISC-V processor is organized into multiple functional units, with each unit responsible for a specific part of instruction execution. The design includes instruction fetching, decoding, arithmetic and logical operations, register operations, branching, jumping, memory access, and processor-level integration.

The project provides hands-on experience with:

* RTL design
* Modular hardware architecture
* RISC-V instruction execution
* SystemVerilog
* Processor datapath design
* Digital hardware verification
* Simulation and waveform analysis

---

# Digital Logic Modules

## 1. Combinational Logic

The `1_combinational` directory contains basic combinational RTL designs.

### Modules

* **4-Input Adder**

  * Performs addition involving four inputs.

* **Hamming Code Encoder**

  * Generates Hamming-code parity information for error detection and correction.

* **Hamming Code Decoder**

  * Decodes Hamming-coded data and identifies/corrects single-bit errors.

* **Programmable Adder/Subtractor**

  * Performs addition or subtraction depending on the selected operation.

* **Multiplexer**

  * Selects one input from multiple inputs based on select signals.

* **Sorting Box**

  * Implements sorting logic using combinational hardware.

* **Trailing One Detector**

  * Detects the position/pattern of trailing `1` bits in a binary input.

---

# 2. Sequential Logic

The `2_sequential` directory contains clock-dependent digital circuits.

### Modules

* **D Flip-Flop**

  * Basic sequential storage element.

* **Clock Divider**

  * Generates a divided-frequency clock signal.

* **FIFO**

  * Implements First-In First-Out data storage.

* **FIR Filter**

  * Hardware implementation of a Finite Impulse Response filter.

* **Moving Average Filter**

  * Computes a moving average of incoming data samples.

* **Sequence Detector**

  * Detects a predefined digital sequence using sequential logic.

* **USB Decoder**

  * Implements decoding logic for USB-related serial data.

---

# 3. RISC-V Processor

The `3_riscv` directory contains the SystemVerilog RTL implementation of the RISC-V processor.

The processor is divided into multiple modules to keep the design modular and easier to understand, simulate, and debug.

## Processor Architecture

```text
                     ┌───────────────────┐
                     │     Processor     │
                     │   processor.sv    │
                     └─────────┬─────────┘
                               │
              ┌────────────────┼────────────────┐
              │                │                │
              ▼                ▼                ▼
        ┌──────────┐     ┌──────────┐     ┌──────────┐
        │   IFU    │────►│   IDU    │────►│   IEU    │
        │ ifu.sv   │     │ idu.sv   │     │ ieu.sv   │
        └──────────┘     └────┬─────┘     └────┬─────┘
                              │                │
                              │                │
                        ┌─────▼─────┐     ┌────▼─────┐
                        │ Register  │     │   ALU    │
                        │   File    │     │ alu.sv   │
                        │ regfile.sv│     │alu_core  │
                        └───────────┘     └────┬─────┘
                                              │
                              ┌───────────────┼───────────────┐
                              │               │               │
                              ▼               ▼               ▼
                         ┌────────┐      ┌────────┐      ┌────────┐
                         │ Branch │      │  Jump  │      │ Memory │
                         │branch.sv│     │jump.sv │      │ mem.sv │
                         └────────┘      └────────┘      └───┬────┘
                                                             │
                                                       ┌─────┴─────┐
                                                       │           │
                                                       ▼           ▼
                                                  ┌────────┐  ┌────────┐
                                                  │  Load  │  │ Store  │
                                                  │load.sv │  │store.sv│
                                                  └────────┘  └────────┘
```

---

# RISC-V Modules

## `processor.sv`

### Top-Level Processor

The top-level processor module integrates the major processor components and connects the instruction path, execution path, register file, control logic, and memory interface.

---

## `ifu.sv`

### Instruction Fetch Unit

The Instruction Fetch Unit is responsible for fetching instructions and managing the instruction address/program counter flow.

The IFU forms the beginning of the processor instruction-execution path.

---

## `idu.sv`

### Instruction Decode Unit

The Instruction Decode Unit is responsible for decoding the fetched RISC-V instruction and generating the required control and operand information for execution.

The IDU works together with the instruction-specific decoder modules.

---

## `ieu.sv`

### Integer Execution Unit

The Integer Execution Unit handles the execution of integer instructions and connects the decoded instruction information with arithmetic/logic and register operations.

---

## `alu.sv`

### Arithmetic Logic Unit

The ALU performs arithmetic and logical operations required by the processor.

Operations handled by the ALU include:

* Addition
* Subtraction
* Logical AND
* Logical OR
* Logical XOR
* Comparison
* Shift operations

---

## `alu_core.sv`

Contains the core ALU operation logic used by the processor's execution path.

Separating the ALU core from the surrounding ALU interface provides a modular structure for the arithmetic and logical datapath.

---

## `regfile.sv`

### Register File

Implements the processor's integer register storage.

The register file provides operands required by instructions and stores results generated during execution.

---

# Instruction Decoders

The processor contains separate decoder modules for different RISC-V instruction formats and operation types.

## `decode_reg_inst.sv`

Decodes register-register instructions.

These instructions operate using values stored in the integer register file.

---

## `decode_imm_inst.sv`

Decodes immediate-type instructions.

Immediate instructions use a constant value encoded directly inside the instruction.

---

## `decode_load_inst.sv`

Decodes load instructions and generates the required control information for memory read operations.

---

## `decode_store_inst.sv`

Decodes store instructions and generates the required control information for memory write operations.

---

## `decode_branch_inst.sv`

Decodes conditional branch instructions.

The decoded information is used by the branch unit to determine whether the processor should change its normal sequential execution flow.

---

## `decode_jump_inst.sv`

Decodes jump instructions and generates the control information required by the jump unit.

---

## `decode_upperimm_inst.sv`

Decodes upper-immediate instruction types and generates the corresponding immediate/control information.

---

# Control Flow Units

## `branch.sv`

### Branch Unit

Handles conditional branch operations.

The branch unit evaluates branch conditions and generates the required control information for changing the instruction flow.

---

## `jump.sv`

### Jump Unit

Handles jump operations and calculates the required target address/control information for transferring program execution.

---

# Memory System

## `mem.sv`

### Memory Module

Implements the processor memory interface/storage logic used by the design.

It provides the memory-side functionality required by instruction and data operations.

---

## `load.sv`

### Load Unit

Handles load operations from memory.

The Load Unit processes the information required to obtain data from the memory system and provide it to the processor datapath.

---

## `store.sv`

### Store Unit

Handles store operations.

The Store Unit generates the required memory write information for transferring data from the processor to memory.

This module forms the data-write path between the processor execution system and the memory system.

---

## `inst_data_arbiter.sv`

### Instruction/Data Arbiter

Controls access between instruction-related and data-related memory requests.

This module manages the interaction between the processor's instruction and data paths when they share memory resources.

---

# Processor Definitions

## `processor_defines.sv`

Contains shared processor definitions used by the RISC-V RTL modules.

Keeping common definitions in a separate file improves consistency and makes the processor design easier to maintain.

---

# RISC-V Instruction Flow

The general instruction flow of the processor can be represented as:

```text
              Instruction Fetch
                      │
                      ▼
               ┌─────────────┐
               │     IFU     │
               └──────┬──────┘
                      │
                      ▼
              Instruction Decode
                      │
                      ▼
               ┌─────────────┐
               │     IDU     │
               └──────┬──────┘
                      │
          ┌───────────┴───────────┐
          │                       │
          ▼                       ▼
   Instruction              Register /
    Decoders                Immediate
          │                   Operands
          └───────────┬───────────┘
                      │
                      ▼
              Integer Execution
                      │
                      ▼
                  ┌───────┐
                  │  ALU  │
                  └───┬───┘
                      │
          ┌───────────┼───────────┐
          │           │           │
          ▼           ▼           ▼
       Register     Branch       Jump
        Result       Logic       Logic
          │
          ▼
      Memory Access
          │
       ┌──┴────┐
       ▼       ▼
     Load    Store
       │       │
       └───┬───┘
           │
           ▼
       Write Back
           │
           ▼
      Register File
```

---

# Instruction Types

The processor design contains dedicated decoding logic for several RISC-V instruction categories:

| Instruction Type             | Decoder                   |
| ---------------------------- | ------------------------- |
| Register instructions        | `decode_reg_inst.sv`      |
| Immediate instructions       | `decode_imm_inst.sv`      |
| Load instructions            | `decode_load_inst.sv`     |
| Store instructions           | `decode_store_inst.sv`    |
| Branch instructions          | `decode_branch_inst.sv`   |
| Jump instructions            | `decode_jump_inst.sv`     |
| Upper-immediate instructions | `decode_upperimm_inst.sv` |

---

# Technologies Used

* **SystemVerilog**
* **Verilog HDL**
* **Python**
* **Cocotb**
* **Icarus Verilog**
* **GTKWave**
* **Visual Studio Code**
* **Ubuntu Linux**
* **Git**
* **GitHub**

---

# Verification and Simulation

The RTL modules can be simulated using HDL simulation tools.

A typical verification workflow is:

```text
SystemVerilog RTL
       │
       ▼
   Testbench
       │
       ▼
 Icarus Verilog
       │
       ▼
   Simulation
       │
       ▼
    VCD File
       │
       ▼
    GTKWave
       │
       ▼
 Waveform Analysis
```

GTKWave can be used to inspect internal signals and verify the behavior of the implemented hardware.

---

# How to Run

Navigate to the directory containing the required module and testbench and execute:

```bash
make
```

To view the generated waveform:

```bash
gtkwave dump.vcd
```

The exact `make` targets and simulation files depend on the module being tested.

---

# Skills Demonstrated

This project demonstrates practical experience in:

* RTL Design
* SystemVerilog
* Verilog HDL
* Digital Logic Design
* Combinational Circuit Design
* Sequential Circuit Design
* Processor Architecture
* RISC-V Architecture
* Instruction Decoding
* ALU Design
* Register File Design
* Memory Interface Design
* Branch and Jump Logic
* Load/Store Operations
* Modular Hardware Design
* Functional Verification
* Cocotb-based Verification
* HDL Simulation
* GTKWave Waveform Debugging
* Linux-based Development
* Git and GitHub

---

# Project Highlights

### Digital Design

Implemented fundamental combinational and sequential circuits to build a strong foundation in RTL and digital hardware design.

### Processor Design

Developed modular components of a **32-bit RISC-V processor**, including instruction fetch, decode, execution, register operations, control-flow operations, and memory access.

### Modular RTL

The processor is divided into independent SystemVerilog modules, making individual blocks easier to understand, simulate, debug, and reuse.

### Memory Operations

Implemented dedicated **Load and Store Units** for handling processor-to-memory and memory-to-processor data transfers.

### Verification

Simulation and waveform analysis are used to examine RTL behavior and debug hardware functionality.

---

# Repository Organization

```text
RISC-V
│
├── 1_combinational
│   └── Basic combinational RTL designs
│
├── 2_sequential
│   └── Clocked/sequential RTL designs
│
└── 3_riscv
    └── RISC-V processor RTL
        │
        ├── Instruction Fetch
        ├── Instruction Decode
        ├── Instruction Decoders
        ├── Integer Execution
        ├── ALU
        ├── Register File
        ├── Branch Unit
        ├── Jump Unit
        ├── Load Unit
        ├── Store Unit
        ├── Memory
        ├── Instruction/Data Arbiter
        ├── Processor Definitions
        └── Processor Top Level
```

---

## Conclusion

This project provides a complete RTL design workflow, starting from basic digital circuits and progressing toward a modular **RISC-V processor implementation using SystemVerilog**.

It combines digital logic design, processor architecture, RTL development, instruction decoding, memory operations, simulation, verification, and waveform-based debugging into a single hardware design project.
