# 🏧 ATM Simulation — Digital Electronics Project

A fully functional **ATM machine simulation** designed and implemented in **Logisim**, built as a Digital Electronics course project. The circuit simulates core ATM operations including PIN verification, account balance display, and debit/credit transactions using fundamental digital components.

---

## 📌 Table of Contents

- [About the Project](#about-the-project)
- [Features](#features)
- [Circuit Components Used](#circuit-components-used)
- [Circuit Architecture](#circuit-architecture)
- [How to Run](#how-to-run)
- [How to Use the Simulation](#how-to-use-the-simulation)
- [Sub-Circuits](#sub-circuits)
- [Author](#author)

---

## 📖 About the Project

This project simulates the working of an **ATM machine** at the digital logic level using **Logisim 2.7.1**. It models real-world ATM functionality — PIN entry, PIN validation, balance display, and debit/credit operations — entirely through logic gates, registers, RAM, comparators, and custom sub-circuits. No microprocessor or software is involved; everything is built from pure digital hardware logic.

---

## ✨ Features

- **PIN Entry System** — 4-digit PIN input using on-screen digit buttons (0–9) with a dedicated CLEAR button to reset entry
- **PIN Verification** — Compares entered PIN against stored PIN using digital comparators; lights a green LED for correct PIN and a red LED for incorrect PIN
- **Balance Display** — Current account balance shown on Hex Digit Displays in real time using RAM-stored values
- **Credit Operation** — Add funds to the account using BCD adder circuits
- **Debit Operation** — Subtract funds from the account using a custom Debit sub-circuit with subtractor logic
- **Mode Selection** — Multiplexer-based selector to switch between Debit (1) and Credit (0) modes
- **BCD Arithmetic** — Balance stored and computed in Binary Coded Decimal (BCD) format for accurate digit-by-digit display
- **Multi-digit Balance Display** — 8 Hex Digit Displays for showing a full multi-digit balance

---

## ⚙️ Circuit Components Used

| Component | Purpose |
|---|---|
| **Logic Gates** (AND, OR, NOT) | PIN verification logic, debit enable/disable control |
| **Registers** (R1, R2, R3, R4) | Temporarily holding each digit of the entered 4-digit PIN |
| **RAM** (2-address, 4-bit & 32-bit) | Storing PIN digits and account balance |
| **Comparators** (4-bit) | Comparing entered PIN digits with stored PIN digits |
| **Subtractor** (32-bit) | Computing balance after debit transaction |
| **BCD Adder** (custom sub-circuit) | Adding digits in BCD format for credit transactions |
| **Multiplexer** (32-bit, 2-to-1) | Selecting between credit and debit output to update balance |
| **Demultiplexer** | Routing PIN digit input to the correct register |
| **Hex Digit Displays** | Displaying PIN digits and account balance |
| **LEDs** (Green & Red) | Visual feedback for correct/incorrect PIN |
| **Buttons** (0–9, CLEAR) | User input for PIN and amount entry |
| **Clock** | Synchronising register writes and RAM operations |
| **Counter** (2-bit) | Tracking which PIN digit position is being entered |
| **Splitter / Bit Extender** | Bus manipulation and bit-width conversion |
| **Tunnels** | Clean wire routing across the circuit |

---

## 🏗️ Circuit Architecture

The project consists of **three circuits**:

### 1. `main` — Top-level ATM Circuit
The main circuit integrates all subsystems:
- A **keypad input** section (buttons 0–9 + CLEAR) feeds digits onto a 4-bit BUS
- A **2-bit counter + demultiplexer** routes each digit to one of four registers (R1–R4), capturing the 4-digit PIN sequentially
- Four **4-bit comparators** compare each stored register digit against the PIN stored in RAM
- A **4-input AND gate** produces a single "Correct PIN" signal — all four digits must match
- A **NOT gate** drives the "Incorrect PIN" red LED
- Two **32-bit RAM modules** store the account balance
- **BCD Adder chains** handle credit (addition) operations digit by digit
- A **Debit sub-circuit** chain handles withdrawal (subtraction) operations
- A **32-bit multiplexer** selects the updated balance from either the credit or debit path and writes it back to RAM
- **8 Hex Digit Displays** show the full account balance at all times

### 2. `Debit` — Debit Sub-Circuit
A reusable combinational logic block that:
- Takes the current balance (4-bit input) and a debit amount
- Uses **AND, OR, NOT gates** to implement BCD subtraction logic
- Outputs the resulting 4-bit digit after deduction
- Multiple instances of this sub-circuit are chained together for multi-digit debit operations

### 3. `BCD-adder` — BCD Adder Sub-Circuit
A custom BCD addition circuit that:
- Takes two 4-bit BCD digits as input
- Uses a primary **4-bit adder** for initial binary addition
- Uses **AND gates** to detect when the result exceeds 9 (invalid BCD)
- Applies a correction by adding 6 (0110) when needed
- Outputs a valid BCD digit and a carry for the next digit

---

## ▶️ How to Run

### Prerequisites
- Download and install **Logisim 2.7.1** from [http://www.cburch.com/logisim/](http://www.cburch.com/logisim/)
- Java Runtime Environment (JRE) is required to run Logisim

### Steps
1. Clone or download this repository
2. Open **Logisim**
3. Go to **File → Open** and select `Digital_Electronics-ATM_simulation.circ`
4. The main ATM circuit will load automatically
5. Click **Simulate → Simulation Enabled** to start the simulation
6. Use the **Poke Tool** (hand icon) to interact with buttons and inputs

---

## 🖱️ How to Use the Simulation

1. **Enter your PIN** — Click digit buttons (0–9) one by one to enter each of the 4 PIN digits. The counter automatically advances to the next digit position after each entry.
2. **Verify PIN** — The comparators check your entry against the stored PIN in real time. The **green LED** lights up if the PIN is correct; the **red LED** lights up if incorrect.
3. **Select operation** — Toggle the **Credit/Debit selector** (0 = Credit, 1 = Debit)
4. **Enter amount** — Input the transaction amount using the digit buttons
5. **View balance** — The updated balance is displayed on the Hex Digit Displays immediately
6. **Clear** — Press the **CLEAR** button to reset the PIN entry registers

---

## 🔌 Sub-Circuits

| Sub-Circuit | Description |
|---|---|
| `BCD-adder` | Adds two BCD digits with carry correction; used for credit transactions |
| `Debit` | Combinational subtraction logic for debit transactions; multiple instances chained for multi-digit operation |
