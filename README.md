# VGA Pong on FPGA

This repository contains my implementation of a hardware Pong game on an FPGA, using a VGA-style
timing generator that is converted to HDMI on the Boolean Board. The design is written in
SystemVerilog and follows a hierarchical RTL design structure.

The project demonstrates:
- A **VGA timing generator** (`vga`) producing 800×600 @ 60 Hz style timing
- A **Pong game datapath and FSM** (`pong`, `pong_fsm`, `ball`, `leftPaddle`, `rightPaddle`)
- A reusable **component library** (comparators, counters, range checks, etc.)
- Integration with the **Boolean Board** HDMI and seven-segment displays

---

## Features

- 800×600 display region with correct HS/VS timing and blanking
- Two paddles (left and right) with constrained vertical motion and no wraparound
- 4×4 pixel ball that bounces off walls and paddles, with discrete horizontal/vertical velocity steps 
- Scoring logic that increments when the ball passes the left or right edge and drives the 7-segment displays
- On-screen color scheme:
  - Left paddle: yellow
  - Right paddle: cyan
  - Ball: white
  - Court boundary lines: green
  - Background: black

---

## Controls (Boolean Board)

- **BTN[0]** – Global reset  
- **BTN[3]** – Serve / start a new rally (ball leaves center and starts moving) 
- **Left paddle**  
  - `SW[14]` – enable movement  
  - `SW[15]` – direction (up/down)  
- **Right paddle**  
  - `SW[1]` – enable movement  
  - `SW[0]` – direction (up/down)  

Scores are shown on the 7-segment displays:
- Left player score → HEX1  
- Right player score → HEX0 

---

## Repository Layout

```text
src/
  chipInterface.sv    # Top-level: clocks, buttons/switches, HDMI, scoring display
  vga.sv              # VGA timing generator (HS, VS, blank, row, col)
  pong.sv             # Pong datapath, FSM, ball & paddles, color generation
  library.sv          # Reusable components (Comparator, Counter, RangeCheck, OffsetCheck, ...)
  seven_segment.sv    # Seven-segment display driver and hex decoding

sim/
  library_tests.sv    # Unit tests for library components
  vga_tb.sv           # (optional) VGA timing testbench
  pong_tb.sv          # (optional) Pong behavior testbench

doc/
  Lab4_VGA_Pong.pdf   # Original lab handout/spec
  design_notes.md     # FSM diagrams, datapath sketches, etc. (optional)

constraints/
  pong.xdc            # FPGA pin/clock constraints (if applicable)
