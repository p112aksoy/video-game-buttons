#  Dual-Processor Video Game Push-Button Controller (DMC8)

This project implements a **video game push-button controller system** using the **DEEDS simulator** and the **DMC8 microprocessor**.  
The system is designed with a **dual-processor architecture** to model a realistic game console where user inputs are processed and safely transferred between two independent microcontrollers.

---

##  Project Overview

- **CPU1 (Transmitter)** acts as the **game controller**
- **CPU2 (Receiver)** acts as the **game engine / actuator**
- Communication between the two CPUs is achieved using a **bidirectional busy–strobe handshaking mechanism**
- This ensures **synchronized, reliable, and loss-free data transfer**

---

##  System Description

### CPU1 – Transmitter (Game Controller)

CPU1 reads player inputs from push-buttons including:

- GO-STRAIGHT  
- GO-BACK  
- GO-LEFT  
- GO-RIGHT  
- STOP *(named **SP_SR** on the console)*  
- JUMP / DOWN  

CPU1 processes these inputs using **polling and edge-detection logic** to prevent repeated commands.

- **STOP (SP_SR)** is implemented as a **high-priority polled state**
- **JUMP / DOWN** is handled using a **hardware interrupt** for immediate response

After processing, CPU1 sends **a single valid command** to CPU2 using **bidirectional handshaking**.

---

### CPU2 – Receiver (Game Engine)

CPU2 receives commands from CPU1 and controls a **movement peripheral implemented with LEDs**.

CPU2 operation includes:

- Detecting the **rising edge of the STROBE signal**
- Asserting **BUSY** before reading incoming data
- Latching the command safely
- Releasing **BUSY** after processing

This guarantees that **each command is received exactly once**.

---

##  Communication Technique

### Bidirectional Handshaking (Main Technique)

- **DATA:** CPU1 OB → CPU2 IB  
- **STROBE:** CPU1 OA bit 0 → CPU2 IA bit 0  
- **BUSY:** CPU2 OA bit 7 → CPU1 IA bit 7  

The handshaking protocol ensures:

- No data loss  
- No overwriting  
- Safe asynchronous operation  

---

##  Additional Techniques Used

### Polling
- CPU1 polls the **BUSY** signal before sending data
- CPU2 polls the **STROBE** signal and applies **edge detection**

### Interrupts
- CPU1 uses a **hardware interrupt** to handle **JUMP / DOWN** actions
- The interrupt routine checks the **STOP (SP_SR)** state to ensure STOP always has priority

---

##  Peripheral Devices

- Push-buttons for video game input  
- LED-based movement output  
- Handshake lines (**BUSY** and **STROBE**)  

All peripherals are connected using **memory-mapped I/O**.

---

##  Project Originality

This project extends beyond basic reference examples by:

- Using a **dual-processor architecture**
- Implementing **parallel command transfer** instead of serial communication
- Applying **bidirectional handshaking**
- Combining **polling and interrupts**
- Introducing **edge detection and priority handling**
- Modeling a **realistic video game control scenario**

---

##  Tools and Technologies

- DEEDS Simulator  
- DMC8 Microprocessor  
- Assembly Language (DMC8)

---

##  Notes

- On the game console interface, the **STOP** function is labeled as **SP_SR**
- All other button names match their logical command definitions
