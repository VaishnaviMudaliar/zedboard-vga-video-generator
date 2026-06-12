# ZedBoard VGA Video Generator

A simple FPGA-based VGA video generator for the **ZedBoard (Zynq-7000)** implemented in **Vivado IP Integrator**. The design generates video data using a custom AXI4-Stream source and displays it through the VGA interface.

## Overview

This project demonstrates:

- Clock generation using Vivado Clocking Wizard
- System reset synchronization
- Custom AXI4-Stream video source (`dataGen`)
- Video timing generation using Video Timing Controller (VTC)
- AXI4-Stream to Video Output conversion
- RGB signal extraction for VGA output
- VGA synchronization signal generation

The design outputs 24-bit RGB video through the ZedBoard VGA interface.

---

## Block Diagram

![Block Diagram](docs/block_diagram.png)

### Main Components

| Module | Description |
|----------|------------|
| `clk_wiz` | Generates the required pixel clock |
| `proc_sys_reset` | Handles reset synchronization |
| `dataGen` | Custom AXI4-Stream video data generator |
| `v_tc` | Video Timing Controller |
| `v_axi4s_vid_out` | Converts AXI4-Stream video to parallel VGA signals |
| `mux` | Video data selection/control |
| `xlslice` | Extracts RGB channels from 24-bit pixel data |

---

## System Architecture

```text
System Clock
     │
     ▼
 Clock Wizard
     │
     ▼
  dataGen ── AXI4-Stream ──► AXI4S Video Out
                                 │
                                 ▼
                        VGA RGB + HSYNC + VSYNC
```

### Video Path

1. `dataGen` generates 24-bit RGB pixel data.
2. Data is streamed using AXI4-Stream protocol.
3. `Video Timing Controller` generates synchronization timing.
4. `AXI4S Video Out` converts the stream into VGA-compatible signals.
5. RGB channels are separated and routed to the VGA connector.

---

## Interfaces

### Inputs

| Signal | Description |
|----------|------------|
| `sys_clock` | Board system clock |
| `reset` | Active-high system reset |

### Outputs

| Signal | Width |
|----------|-------|
| `VGA_R` | 4 bits |
| `VGA_G` | 4 bits |
| `VGA_B` | 4 bits |
| `vid_hsync` | 1 bit |
| `vid_vsync` | 1 bit |

---

## AXI4-Stream Signals

The custom video generator implements the standard AXI4-Stream Video interface:

| Signal | Description |
|----------|------------|
| `tdata[23:0]` | RGB pixel data |
| `tvalid` | Pixel data valid |
| `tready` | Downstream ready |
| `tuser` | Start of frame |
| `tlast` | End of line |

---

## Requirements

### Hardware

- ZedBoard (Zynq-7000 XC7Z020)
- VGA monitor
- VGA cable

### Software

- Vivado 20XX.X (replace with your version)
- Xilinx IP Catalog

---

## Building the Design

1. Clone the repository:

```bash
git clone https://github.com/<username>/<repo-name>.git
```

2. Open Vivado.

3. Open the project:

```text
File → Open Project
```

4. Generate the bitstream:

```text
Flow Navigator → Generate Bitstream
```

5. Program the FPGA:

```text
Hardware Manager → Program Device
```

---

## Repository Structure

```text
.
├── src/
│   ├── dataGen.v
│   ├── mux.v
│   └── ...
├── bd/
│   └── design.bd
├── constraints/
│   └── zedboard.xdc
├── docs/
│   └── block_diagram.png
└── README.md
```

---

## Future Improvements

- Test pattern generation
- Frame buffer support using DDR memory
- HDMI output
- Dynamic resolution selection
- Hardware sprites and graphics acceleration

---

## Example Output

Add screenshots of the VGA output here.

```text
docs/output_image.jpg
```

---

## License

MIT License

