# D Flip-Flop Verification using SystemVerilog
This project demonstrates **verification of a D Flip-Flop (D-FF)** using **SystemVerilog OOP concepts** (classes, transactions, mailboxes, events) and a structured **testbench environment**.
The RTL implements a simple positive-edge triggered D flip-flop with synchronous reset, while the testbench uses a layered verification environment with generator, driver, monitor, and scoreboard.

##  RTL Design (D Flip-Flop)
**File:** `D_ff_Rtl.sv`
The RTL module `d_ff` is connected via a virtual interface `d_ff_if`.

On the **posedge of clk**:
   If `reset = 1`, output `q = 0`.
    Else, `q` follows input `d`.

### **Interface Signals**
| Signal  | Direction | Type  | Description       |
| ------- | --------- | ----- | ----------------- |
| `clk`   | Input     | logic | Clock signal      |
| `reset` | Input     | logic | Active-high reset |
| `d`     | Input     | logic | Data input        |
| `q`     | Output    | logic | Data output       |

##  Verification Environment
**File:** `D_ff_Tb.sv`

The testbench is built in a **modular UVM-like style** without using UVM library. It includes:

### **Components**
1. **Transaction**
    Holds stimulus (`d`) and response (`q`).
    Provides `copy()` and `display()` methods.

2. **Generator**
   * Creates randomized transactions (`d`).
   * Sends them to driver and scoreboard reference mailbox.

3. **Driver**
    Drives transaction `d` values to DUT via the virtual interface.
    Handles reset sequence.

4. **Monitor**
    Observes DUT output (`q`) and sends data to scoreboard.

5. **Scoreboard**
    Compares monitored output with expected reference data.
    Reports **MATCHED** or **MISMATCHED** results.

6. **Environment**
   Instantiates generator, driver, monitor, and scoreboard.
    Connects components via mailboxes and events.
    Controls simulation flow (pre-test, test, post-test).

##  Simulation Flow

1. **Reset Phase**: Reset DUT for 5 cycles.
2. **Test Phase**:
    Generator creates `N` randomized `d` inputs.
    Driver drives them into DUT.
    Monitor captures DUT’s output `q`.
    Scoreboard checks if `q == d` (expected D-FF behavior).
3. **Post-Test**:
    Wait until all transactions are verified.
    End simulation.

