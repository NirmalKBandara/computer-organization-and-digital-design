---
# Combinational Logic Elements & Systems II
---
> *"In the middle of difficulty lies opportunity."*  
> — Albert Einstein

## Introduction

Building upon basic combinational logic elements, this module explores sophisticated digital components that handle data routing, encoding, and interfacing. Decoders, encoders, multiplexers, and three-state devices form the backbone of modern digital systems, enabling everything from memory addressing to bus management.

### The Role of Data Management Circuits

While arithmetic circuits process data, we also need circuits that:
- **Route data** between different parts of a system
- **Convert between** different data representations
- **Share resources** among multiple components
- **Interface** with memory and I/O devices

## Decoders

Decoders are fundamental circuits that convert encoded information into a more accessible format. They act as "translators" in digital systems, enabling selective activation of components.

### Definition and Characteristics

A decoder is a combinational circuit that:
- Converts binary information from **n input lines** to a maximum of **$2^n$ unique output lines**
- Activates exactly **one output** for each input combination
- Generates all possible **minterms** of the input variables

**Mathematical Representation:**
For an n-to-$2^n$ decoder with inputs $I_{n-1}, ..., I_1, I_0$:

$$ Y_i = m_i = \prod_{j=0}^{n-1} I_j^{b_j} $$

Where:
- $i$ is the output line number (0 to $2^n-1$)
- $b_j$ is bit $j$ of the binary representation of $i$
- $I_j^{b_j} = I_j$ if $b_j = 1$, or $I_j'$ if $b_j = 0$

### 2-to-4 Decoder

**Truth Table:**

| EN | I₁ | I₀ | Y₃ | Y₂ | Y₁ | Y₀ |
|----|----|----|----|----|----|----|
| 0  | x  | x  | 0  | 0  | 0  | 0  |
| 1  | 0  | 0  | 0  | 0  | 0  | 1  |
| 1  | 0  | 1  | 0  | 0  | 1  | 0  |
| 1  | 1  | 0  | 0  | 1  | 0  | 0  |
| 1  | 1  | 1  | 1  | 0  | 0  | 0  |

**Boolean Expressions:**
$$ Y_0 = EN \cdot I_1' \cdot I_0' $$
$$ Y_1 = EN \cdot I_1' \cdot I_0 $$
$$ Y_2 = EN \cdot I_1 \cdot I_0' $$
$$ Y_3 = EN \cdot I_1 \cdot I_0 $$

**Key Features:**
- Enable (EN) input for cascading and control
- Active-high outputs (can be made active-low with NAND gates)
- Each output represents one minterm

### 3-to-8 Decoder

For larger decoders, the pattern extends naturally:

**Output Equations:**
$$ D_0 = E \cdot A_2' \cdot A_1' \cdot A_0' $$
$$ D_1 = E \cdot A_2' \cdot A_1' \cdot A_0 $$
$$ D_2 = E \cdot A_2' \cdot A_1 \cdot A_0' $$
$$ ... $$
$$ D_7 = E \cdot A_2 \cdot A_1 \cdot A_0 $$

### Decoder Expansion

Large decoders can be built hierarchically using smaller decoders.

#### Hierarchical Design Principle

To build an n-to-$2^n$ decoder:
1. Use smaller decoders as building blocks
2. Higher-order bits select which decoder is active
3. Lower-order bits drive the selected decoder

#### Example: 3-to-8 Decoder Using 2-to-4 and 1-to-2 Decoders

**Architecture:**
- MSB (A₂) connects to 1-to-2 decoder
- LSBs (A₁, A₀) connect to both 2-to-4 decoders
- 1-to-2 decoder outputs enable the 2-to-4 decoders

**Connection Details:**
- When A₂ = 0: Enable lower 2-to-4 decoder (outputs D₀-D₃)
- When A₂ = 1: Enable upper 2-to-4 decoder (outputs D₄-D₇)

#### Example: 3-to-8 Decoder Using Two 2-to-4 Decoders

**Direct Method:**
- Use A₂ directly as enable control
- Connect A₂' to EN of first decoder
- Connect A₂ to EN of second decoder

### Implementing Logic Functions with Decoders

Since decoders generate all minterms, any Boolean function can be implemented by ORing the appropriate decoder outputs.

#### Design Process

1. **Identify minterms** where function output is 1
2. **Connect corresponding decoder outputs** to OR gate
3. **Result** is the desired function

#### Example: Implementing F(A,B,C) = Σm(1,3,5,7)

**Solution:**
- Use 3-to-8 decoder
- Connect outputs D₁, D₃, D₅, D₇ to 4-input OR gate
- Output of OR gate is F

**Alternative Implementation:**
For functions with many minterms, it may be more efficient to:
- Implement F' using fewer minterms
- Invert the result

## Encoders

Encoders perform the inverse operation of decoders, converting unencoded information into a coded format.

### Definition and Purpose

An encoder:
- Converts **m-bit input** code to **n-bit output** code
- Where **n ≤ m ≤ $2^n$**
- Typically has **only one active input** at a time
- Generates **binary code** corresponding to active input

### 4-to-2 Encoder

**Truth Table:**

| Y₃ | Y₂ | Y₁ | Y₀ | A₁ | A₀ |
|----|----|----|----|----|----|
| 0  | 0  | 0  | 1  | 0  | 0  |
| 0  | 0  | 1  | 0  | 0  | 1  |
| 0  | 1  | 0  | 0  | 1  | 0  |
| 1  | 0  | 0  | 0  | 1  | 1  |

**Boolean Expressions:**
$$ A_1 = Y_3 + Y_2 $$
$$ A_0 = Y_3 + Y_1 $$

### 8-to-3 Encoder (Octal-to-Binary)

**Output Equations:**
$$ A_2 = Y_7 + Y_6 + Y_5 + Y_4 $$
$$ A_1 = Y_7 + Y_6 + Y_3 + Y_2 $$
$$ A_0 = Y_7 + Y_5 + Y_3 + Y_1 $$

**Pattern Recognition:**
- $A_2$ = 1 when input ≥ 4
- $A_1$ = 1 when input ∈ {2,3,6,7}
- $A_0$ = 1 when input is odd

### Decimal-to-BCD Encoder

Converts decimal digits (0-9) to 4-bit BCD code.

**Output Equations:**
$$ A_3 = Y_9 + Y_8 $$
$$ A_2 = Y_7 + Y_6 + Y_5 + Y_4 $$
$$ A_1 = Y_7 + Y_6 + Y_3 + Y_2 $$
$$ A_0 = Y_9 + Y_7 + Y_5 + Y_3 + Y_1 $$

### Priority Encoder

Standard encoders have a limitation: undefined behavior when multiple inputs are active. Priority encoders solve this by assigning priorities.

**Features:**
- Handles multiple active inputs
- Outputs code of highest-priority active input
- Includes "valid output" indicator
- Essential for interrupt controllers

**4-to-2 Priority Encoder Truth Table:**

| Y₃ | Y₂ | Y₁ | Y₀ | A₁ | A₀ | V |
|----|----|----|----|----|----|---|
| 0  | 0  | 0  | 0  | X  | X  | 0 |
| 0  | 0  | 0  | 1  | 0  | 0  | 1 |
| 0  | 0  | 1  | X  | 0  | 1  | 1 |
| 0  | 1  | X  | X  | 1  | 0  | 1 |
| 1  | X  | X  | X  | 1  | 1  | 1 |

Where V is the valid output indicator.

### Application: Positional Encoder

**Example Use Case:** Rotary shaft position sensing
- Multiple sensors detect shaft angle
- Encoder converts to binary angle representation
- Used in robotics and control systems

## Multiplexers

Multiplexers (MUX) are data selectors that route one of several inputs to a single output, acting as digitally controlled switches.

### Definition and Operation

A multiplexer:
- Has **$2^n$ data inputs**, **n select lines**, and **1 output**
- Routes selected input to output
- Acts as a **digital switch**
- Essential for **time-division multiplexing**

**General Expression:**
For a $2^n$-to-1 MUX with select lines $S_{n-1}...S_0$:

$$ Y = \sum_{i=0}^{2^n-1} I_i \cdot m_i $$

Where $m_i$ is the minterm of select lines corresponding to input $I_i$.

### 2-to-1 Multiplexer

**Truth Table:**

| S | Y |
|---|---|
| 0 | I₀ |
| 1 | I₁ |

**Boolean Expression:**
$$ Y = S' \cdot I_0 + S \cdot I_1 $$

### 4-to-1 Multiplexer

**Select Lines:** S₁S₀

**Truth Table:**

| S₁ | S₀ | Y |
|----|----|---|
| 0  | 0  | I₀ |
| 0  | 1  | I₁ |
| 1  | 0  | I₂ |
| 1  | 1  | I₃ |

**Boolean Expression:**
$$ Y = S_1'S_0'I_0 + S_1'S_0I_1 + S_1S_0'I_2 + S_1S_0I_3 $$

### Implementing Boolean Functions with Multiplexers

Any n-variable Boolean function can be implemented using a $2^{n-1}$-to-1 multiplexer.

#### Method 1: Direct Implementation
Use n-to-1 MUX where n = $2^k$ (k = number of variables)

**Example:** Implement F(A,B,C) = Σm(1,2,3,6,7)

Using 8-to-1 MUX:
- Connect A, B, C to select lines
- Set data inputs: I₀=0, I₁=1, I₂=1, I₃=1, I₄=0, I₅=0, I₆=1, I₇=1

#### Method 2: Reduced MUX Implementation
Use (n-1)-variable MUX for n-variable function

**Procedure:**
1. Use n-1 variables as select lines
2. Express function in terms of remaining variable
3. Connect 0, 1, variable, or variable' to data inputs

**Example:** F(A,B,C) = Σm(1,2,3,6,7) using 4-to-1 MUX

| A B | F in terms of C |
|-----|------------------|
| 00  | C                |
| 01  | 1                |
| 10  | 0                |
| 11  | 1                |

Connect: I₀=C, I₁=1, I₂=0, I₃=1

## Digital Buffers

Buffers are often overlooked but crucial components that manage signal integrity and interfacing in digital systems.

### Single Input Digital Buffer

**Function:** Non-inverting buffer (identity gate)

**Boolean Expression:**
$$ Q = A $$

**Why Use Buffers?**
1. **Signal Amplification:** Restore degraded signals
2. **Isolation:** Prevent loading effects
3. **Fanout Enhancement:** Drive multiple loads
4. **Delay Matching:** Synchronize signal paths

**Implementation:** Two cascaded NOT gates

### Three-State (Tri-State) Buffers

Three-state buffers add a high-impedance state to standard logic levels.

**States:**
1. **Logic 0:** Low voltage output
2. **Logic 1:** High voltage output
3. **High-Z:** High impedance (disconnected)

### Tri-State Buffer Operation

**Truth Table (Active-High Enable):**

| EN | A | Y |
|----|---|---|
| 0  | X | Z |
| 1  | 0 | 0 |
| 1  | 1 | 1 |

**Truth Table (Active-Low Enable):**

| EN̄ | A | Y |
|----|---|---|
| 1  | X | Z |
| 0  | 0 | 0 |
| 0  | 1 | 1 |

### Types of Tri-State Buffers

#### 1. Active-High Tri-State Buffer
- Output enabled when EN = 1
- Standard non-inverting operation

#### 2. Active-High Inverting Tri-State Buffer
- Output enabled when EN = 1
- Inverts input when enabled

#### 3. Active-Low Tri-State Buffer
- Output enabled when EN̄ = 0
- Standard non-inverting operation

#### 4. Active-Low Inverting Tri-State Buffer
- Output enabled when EN̄ = 0
- Inverts input when enabled

### Bus Systems and Tri-State Control

**The Bus Contention Problem:**
When multiple devices connect to a shared bus, simultaneous driving can cause:
- Short circuits (one driving 0, another driving 1)
- Undefined bus states
- Excessive current flow
- Component damage

**Solution: Tri-State Bus Control**

**Design Rules:**
1. **Only one device drives the bus** at any time
2. **All others in high-Z state**
3. **Use decoder** to generate enable signals
4. **Ensure no overlap** in enable timing

### Example: 4-Device Bus System

**Components:**
- 4 devices with tri-state outputs
- 2-to-4 decoder for device selection
- Shared data bus

**Operation:**
```
Select  Active Device  Others
  00        Device 0    High-Z
  01        Device 1    High-Z
  10        Device 2    High-Z
  11        Device 3    High-Z
```

**Timing Considerations:**
- Add delay between disable and enable
- Prevents momentary contention
- Critical for high-speed systems

## Design Applications

### Application 1: Memory Address Decoder

**Problem:** Access one of 1024 memory locations

**Solution:**
- 10-to-1024 decoder (impractical)
- Better: Two-stage decoding
  - 5-to-32 row decoder
  - 5-to-32 column decoder
  - 32×32 = 1024 locations

### Application 2: Seven-Segment Display Decoder

**Function:** Convert 4-bit BCD to seven-segment code

**Truth Table (partial):**

| BCD | a | b | c | d | e | f | g |
|-----|---|---|---|---|---|---|---|
| 0000| 1 | 1 | 1 | 1 | 1 | 1 | 0 |
| 0001| 0 | 1 | 1 | 0 | 0 | 0 | 0 |
| 0010| 1 | 1 | 0 | 1 | 1 | 0 | 1 |
| ... | . | . | . | . | . | . | . |

### Application 3: Data Distribution System

**Using MUX and DEMUX:**
- Multiplexer selects data source
- Demultiplexer routes to destination
- Enables flexible interconnection

### Application 4: Function Generator

**Programmable Logic Using MUX:**
- Store function truth table in MUX inputs
- Change function by changing input connections
- Basis for programmable logic devices

## Advanced Topics

### Cascade and Parallel Expansion

**Decoder Expansion Formula:**
To build an (m+n)-to-$2^{m+n}$ decoder:
- Use $2^m$ of n-to-$2^n$ decoders
- Plus one m-to-$2^m$ decoder for selection

**Multiplexer Expansion:**
To build a $2^{m+n}$-to-1 MUX:
- Use $2^m$ of $2^n$-to-1 multiplexers
- Plus one $2^m$-to-1 multiplexer for output

### Hazards in Combinational Circuits

**Issue:** Output glitches during input transitions

**Solution for MUX/Decoder circuits:**
1. Use Gray code for select lines
2. Add filtering capacitors
3. Synchronize with clock

### Power Optimization

**Techniques:**
1. **Enable control:** Disable unused sections
2. **Clock gating:** Stop switching in idle blocks
3. **Dynamic sizing:** Adjust drive strength

## Historical Note

**Claude Shannon (April 1916 – February 2001)**, in addition to his work on Boolean algebra, also pioneered the use of multiplexing in communication systems. His work on sampling theory showed how multiple analog signals could be combined and transmitted over a single channel, leading to modern time-division multiplexing used in digital systems. This concept directly influenced the development of digital multiplexers and data selectors.

## Key Takeaways

1. **Decoders:** Generate all minterms, enable selective activation
2. **Encoders:** Compress information, priority versions handle conflicts
3. **Multiplexers:** Universal logic elements and data routers
4. **Tri-State Buffers:** Enable bus sharing without conflicts
5. **Hierarchical Design:** Build large systems from smaller blocks

## Common Design Pitfalls

1. **Bus contention** from overlapping tri-state enables
2. **Missing priority logic** in encoder applications
3. **Incorrect decoder sizing** for memory systems
4. **Forgetting pull-up resistors** on tri-state buses
5. **Timing violations** in cascaded circuits

## Practice Problems

1. **Design a 4-to-16 decoder** using 2-to-4 decoders
2. **Implement F(A,B,C,D) = Σm(1,3,5,7,11,13)** using an 8-to-1 MUX
3. **Create a priority encoder** for an 8-level interrupt system
4. **Design a bus arbiter** for 4 devices using tri-state logic
5. **Build a 16-to-4 encoder** with valid output indication

## Summary

Data routing and encoding circuits form the nervous system of digital designs. Decoders activate specific components, encoders compress information, multiplexers route data paths, and tri-state buffers enable resource sharing. Together, these elements create flexible, efficient systems that can adapt to changing data flow requirements. Understanding their operation and applications is crucial for designing everything from simple controllers to complex computer architectures.
