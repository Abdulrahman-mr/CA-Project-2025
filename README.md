# 32-bit Pipelined MIPS Processor (Custom Design)

A complete, custom-designed **32-bit MIPS processor**, implemented from scratch in **Logisim** with full **5-stage pipelining**, **hazard detection**, and a working **2-bit dynamic branch predictor**. This project simulates a real processor architecture — instruction by instruction, stage by stage — just like what's used in modern CPUs.

## Architecture Overview

This processor implements a **RISC MIPS-like architecture**, with:
- **20-bit word-aligned Program Counter (PC)**
- **Separate Instruction and Data Memories** (Harvard architecture)
- Support for **R-type, I-type, and SB-type** instructions
- A fully functional **Register File (32 registers)** with hardwired zero
- Word-addressable **Data Memory** with byte-level access enabled
- **Signed/unsigned operations**, overflow handling, and condition flags

## Pipeline Design

The processor follows a **5-stage pipeline** with stage-separated control:

| Stage | Function                     |
|-------|------------------------------|
| IF    | Instruction Fetch            |
| ID    | Instruction Decode & Register Read |
| EX    | ALU Operation / Branch Logic |
| MEM   | Memory Access (Load/Store)   |
| WB    | Write Back to Register File  |

Each stage is separated by its own pipeline register:  
`IF/ID`, `ID/EX`, `EX/MEM`, and `MEM/WB`, allowing instruction overlap and increased throughput.

## Hazard Detection and Resolution

This processor supports:

- **Data Hazard Detection Unit**
  - Detects RAW and Load-Use hazards
  - Stalling logic to pause pipeline when needed
- **Forwarding Unit**
  - Dynamically selects operand sources from pipeline registers (EX/MEM, MEM/WB) to resolve dependencies
- **Control Hazards**
  - Pipeline flushing on mispredictions or JAL-type jumps

## Branch Prediction Unit

The processor features a **custom 2-bit dynamic branch predictor**:

- **2-bit saturating counter per branch entry**
- **Tag-matched Branch Target Buffer (BTB)**:
  - Stores: `{PC Tag, Target Address, Prediction Counter}`
  - Indexed by lower PC bits
- **Prediction output** guides PC update (PC+1 or Target)
- **Misprediction logic** supports pipeline flush and counter update

This design mimics realistic processor speculation behavior.

## Verification and Testing

The processor has been tested with:

- **Instruction sequences** that stress hazard logic (e.g., dependent ALU ops)
- **Branch-heavy testbenches** to verify prediction and pipeline response
- **Memory load/store correctness**
- **Custom programs** written in MIPS assembly, converted to binary, and loaded via Logisim's RAM

## 📁 File Structure

