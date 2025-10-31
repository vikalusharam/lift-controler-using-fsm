[README_clean.md](https://github.com/user-attachments/files/23268226/README_clean.md)
 Lift Controller using FSM

This project implements a Lift (Elevator) Control System using a Finite State Machine (FSM) in Verilog HDL.  
The system handles lift movement between floors, door operations, and user requests using state transitions.

 Overview

A Finite State Machine (FSM) is used to model the lift’s behavior with the following states:
IDLE: Lift is stationary and waiting for a request.
MOVING_UP: Lift moves upward toward the requested floor.
MOVING_DOWN: Lift moves downward toward the requested floor.
DOOR_OPEN: Lift door opens when it reaches the requested floor.

 Features

- Supports multiple floors (e.g., 4 floors: 0–3)
- Handles floor requests dynamically (up or down direction)
- Automatic door open/close with timer control
- Displays the current floor
- Simple and modular FSM design

 FSM States

| State | Description |
|--------|--------------|
| `IDLE` | Lift waits for a new floor request |
| `MOVING_UP` | Lift moves to a higher floor request |
| `MOVING_DOWN` | Lift moves to a lower floor request |
| `DOOR_OPEN` | Door opens when lift reaches the target floor |

 State Transition Diagram

text
        +----------+
        |   IDLE   |<----------------------+
        +----------+                       |
           |   |   |                       |
     req>curr req<curr req=curr        door_timer_done
           v   v   v                       ^
     +--------+  +--------+  +--------+    |
     |MOVING_UP|  |MOVING_DOWN|  |DOOR_OPEN|
     +--------+  +--------+  +--------+
           |           |           |
           +-----------+-----------+

 Technical Details

- **Language:** Verilog HDL  
- **Concepts Used:** FSM, Sequential and Combinational Logic  
- **Simulation Tools:** ModelSim / Vivado / Quartus Prime  
- **Target Device:** FPGA / Simulation  

 File Structure
Lift-Controller-FSM/
│
├── src/
│   └── lift_controller.v
│
├── simulation/
│   └── testbench.v
│
├── docs/
│   └── FSM_Diagram.png
│
└── README.md

 Working Principle

1. Detects pending floor requests  
2. Determines direction of movement  
3. Moves toward the requested floor  
4. Opens the door at the target floor  
5. Waits for a short duration  
6. Closes the door and returns to IDLE  

Example (Verilog Snippet)

verilog
always @(*) begin
    case (state)
        IDLE: begin
            if (req > (1 << current_floor)) next_state = MOVING_UP;
            else if (req < (1 << current_floor)) next_state = MOVING_DOWN;
            else if (req[current_floor]) next_state = DOOR_OPEN;
        end

        MOVING_UP: if (req[current_floor]) next_state = DOOR_OPEN;
        MOVING_DOWN: if (req[current_floor]) next_state = DOOR_OPEN;

        DOOR_OPEN: if (door_timer_done) next_state = IDLE;
    endcase
end
