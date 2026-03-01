# FPGA FIR Low-Pass Filter on Basys 3

**12th-order FIR filter with sine wave generator for Xilinx Basys 3 FPGA**

```
Sine Generator (ROM) → FIR Filter (1kHz cutoff) → DAC/LEDs
```

## Key Features

- **Filter**: 12th-order low-pass, 1kHz cutoff, 10kHz sampling rate
- **Input**: Sine wave generator with 4 selectable frequencies (600Hz, 1000Hz, 2200Hz, 3900Hz)
- **Clock**: 100MHz system clock
- **Architectures**: Three implementations compared (parallel, 1-stage pipeline, 2-stage pipeline)

## Synthesis Results

| Architecture | LUTs | Flip-Flops | Max Frequency |
|--------------|------|------------|---------------|
| Parallel     | 462  | 136        | 179 MHz       |
| 1-FF Pipeline| 465  | 161        | 219 MHz       |
| 2-FF Pipeline| 462  | 186        | 239 MHz       |

**Insight**: Pipeline architectures trade area (more FFs) for 22-33% higher maximum frequency.

## Quick Start

```bash
# Simulate with GHDL + GTKWave
make

# Or with Nix (reproducible environment)
nix develop
make
```

Simulation runs for 6.754ms and generates waveform output in GTKWave.

## Project Structure

```
code/
├── main.vhd                    # Top-level entity
├── sine_generator.vhd          # ROM-based sine generator
├── rom.vhd                     # 16-sample lookup table
├── filter_parallel.vhd         # Combinational FIR
├── filter_pipeline_simple.vhd  # 1-stage pipeline FIR
├── filter_pipeline.vhd         # 2-stage pipeline FIR
└── Makefile                    # GHDL build system

report/                         # LaTeX report with analysis
analog_lab/                     # Op-amp design (LTSpice)
layout_lab/                     # Ring counter layout (Microwind)
```

## Hardware

- **FPGA**: Xilinx Artix-7 (Basys 3 board)
- **Inputs**: 2-bit frequency select switches, reset button
- **Outputs**: 8-bit signed LEDs, 8-bit unsigned DAC