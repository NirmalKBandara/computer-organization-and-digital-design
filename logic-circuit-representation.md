---
# Logic Circuit Representation and Simplification
---
> *"Logic will get you from A to B. Imagination will take you everywhere."*  
> — Albert Einstein

## Introduction

Digital circuits are the foundation of modern computing systems. At their core, these circuits are abstractions of transistor networks that process binary information. This module explores how we represent, analyze, and simplify logic circuits using various mathematical tools and techniques.

### From Transistors to Logic

Logic circuits provide a high-level abstraction for transistor circuits. Instead of dealing with voltages and currents, we work with logical values (0 and 1) and logical operations (AND, OR, NOT).

**Key Technologies:**
- **TTL (Transistor-Transistor Logic)** 
  - Uses bipolar junction transistors
- **CMOS (Complementary Metal-Oxide-Semiconductor)**
  - Uses field-effect transistors

## Power Consumption in Digital Circuits

Understanding power consumption is crucial for circuit design:

### Dynamic Power Consumption

$$ P_{dynamic} = C \cdot V^2 \cdot f $$

Where:
- *C* = capacitance of the circuit (wires and gates)
- *V* = supply voltage
- *f* = charging frequency of the capacitor

### Static Power Consumption

$$ P_{static} = V \cdot I_{leakage} $$

Where:
- *V* = supply voltage
- $$I_{leakage}$$ = leakage current

### Energy Consumption

$$ E = P \cdot t $$

**Design Note :** In CMOS technology, complex gates like AND are often built using NAND gates followed by NOT gates, optimizing for transistor count and performance.

## Classification of Logic Circuits

### 1. Combinational Logic Circuits
- **No memory** : Output depends only on current inputs
- **No feedback loops** : No path from output back to input
- **Instantaneous response** : Output changes immediately with input

### 2. Sequential Logic Circuits
- **Memory present** : Can store previous states
- **Feedback loops** : Output can influence future behavior
- **State-dependent** : Output depends on both current inputs and previous states

## Combinational Logic Representation

### Methods of Representation

1. **Problem Statement** : Natural language description
2. **Truth Table** : Exhaustive listing of all input-output combinations
3. **Boolean Expression** : Algebraic representation
   - Sum of Products (SOP) form
   - Product of Sums (POS) form
4. **Karnaugh Maps (K-Maps)** : Visual representation for simplification
5. **Logic Circuit Diagram** : Schematic using gate symbols

### Digital Circuit Design Process. 

1.  Identify Inputs and Outputs from problem statement

2. Create Truth Table showing all possible combinations

3. Derive Boolean Expression using standard forms

4. Simplify using Boolean Algebra or K-Maps

5. Draw Logic Circuit Diagram

## Logic Gates

### Basic Gates

| Gate | Symbol | Boolean Expression | Truth Table |
|------|--------|-------------------|-------------|
| AND | · or ∧ | $$Z = A \cdot B$$ | 00→0, 01→0, 10→0, 11→1 |
| OR | + or ∨ | $$Z = A + B$$ | 00→0, 01→1, 10→1, 11→1 |
| NOT | ' or ¯ | $$Z = A'$$ | 0→1, 1→0 |
| NAND | ↑ | $$Z = (A \cdot B)'$$ | 00→1, 01→1, 10→1, 11→0 |
| NOR | ↓ | $$Z = (A + B)'$$ | 00→1, 01→0, 10→0, 11→0 |

### Special Purpose Gates

| Gate | Function | Boolean Expression |
|------|----------|-------------------|
| XOR (Exclusive OR) | Odd Parity Detector | $$Z = A \oplus B = A'B + AB'$$ |
| XNOR (Exclusive NOR) | Even Parity Detector | $$Z = A \odot B = A'B' + AB$$ |

## Truth Tables

For $$N$$ input variables, there are $$2^N$$ possible input configurations.

**Example :** 3 variable function

| A | B | C | Z |
|---|---|---|---|
| 0 | 0 | 0 | 0 |
| 0 | 0 | 1 | 1 |
| 0 | 1 | 0 | 1 |
| 0 | 1 | 1 | 1 |
| 1 | 0 | 0 | 1 |
| 1 | 0 | 1 | 0 |
| 1 | 1 | 0 | 1 |
| 1 | 1 | 1 | 1 |

## Karnaugh Maps (K-Maps)

K-Maps provide a visual method for simplifying Boolean expressions by grouping adjacent 1s (or 0s) in a truth table arranged in Gray code order.

### Building a K-Map

1. Create empty map with variables arranged in Gray code order

2. Fill cells with truth table values

3. Group 1s according to K-Map rules

4. Derive simplified Boolean expression

### K-Map Rules for Grouping

1. **Groups must contain only 1s** (or X for don't care)    

2. **Group size must be $$2^n$$** (1, 2, 4, 8, 16, ...)
3. **Each 1 must be in at least one group**
4. **Groups can be horizontal or vertical** (not diagonal)
5. **Each group should be as large as possible**
6. **Use minimum number of groups**
7. **Groups may overlap**
8. **Groups may wrap around** edges

### Example: 3 Variable K-Map

For the truth table above:

```
 \ BC
A \  00  01  11  10  <== Gray Codes
  0   0   1   1   1
  1   1   0   1   0
```

Groupings yield : $$Z = B + AC'$$

## Canonical Forms

Canonical forms provide unique representations of Boolean functions.

### Two-Level Canonical Forms

Every Boolean function can be uniquely represented in two standard forms:

### 1. Sum of Products (SOP) / Disjunctive Normal Form

**Definition :** Logical sum (OR) of product terms (AND), where each product term is a minterm.

**Minterm :** Product term containing all variables (complemented or non-complemented)

**Example :** $$F(A,B,C) = A'B'C + A'BC + AB'C' + ABC' + ABC$$

**Notation :** $$F(A,B,C) = \sum m(1,3,4,6,7)$$

### 2. Product of Sums (POS) / Conjunctive Normal Form

**Definition :** Logical product (AND) of sum terms (OR), where each sum term is a maxterm.

**Maxterm :** Sum term containing all variables (complemented or non-complemented)

**Example :** $$F(A,B,C) = (A+B+C')(A+B'+C)(A'+B+C')$$

**Notation :** $$F(A,B,C) = \prod M(0,2,5)$$

### Duality Principle

For any Boolean function:

$$ \sum m(minterms) = \prod M(maxterms) $$

Where the minterm indices and maxterm indices are complementary sets.

## Minterms and Maxterms

### Definitions

| Row | A B C | Minterm | Symbol | Maxterm | Symbol |
|-----|-------|---------|--------|---------|--------|
| 0 | 0 0 0 | A'B'C' | m₀ | A+B+C | M₀ |
| 1 | 0 0 1 | A'B'C | m₁ | A+B+C' | M₁ |
| 2 | 0 1 0 | A'BC' | m₂ | A+B'+C | M₂ |
| 3 | 0 1 1 | A'BC | m₃ | A+B'+C' | M₃ |
| 4 | 1 0 0 | AB'C' | m₄ | A'+B+C | M₄ |
| 5 | 1 0 1 | AB'C | m₅ | A'+B+C' | M₅ |
| 6 | 1 1 0 | ABC' | m₆ | A'+B'+C | M₆ |
| 7 | 1 1 1 | ABC | m₇ | A'+B'+C' | M₇ |

### Conversion Between Forms

**SOP to Canonical SOP:**

$$ AB + B' = AB(C+C') + (A+A')B'(C+C') $$

$$ = ABC + ABC' + AB'C + AB'C' + A'B'C + A'B'C' $$

$$ = m_7 + m_6 + m_5 + m_4 + m_1 + m_0 $$

**POS to Canonical POS:**

$$ (A+B) \cdot A $$

$$ = (A+B+CC') \cdot (A+BB'+CC') $$

$$ = (A+B+C)(A+B+C') \cdot (A+B)(A+B')(A+C)(A+C') $$

## Standard Forms

Standard forms are simplified versions of canonical forms where terms don't necessarily contain all variables.

### Sum of Products (SOP)
- Each term can be a single literal or product of literals
- Example : $$F = A + BC' + A'BC$$

### Product of Sums (POS)
- Each term can be a single literal or sum of literals
- Example: $$F = (A+B)(A'+C)(B+C')$$

## Implementation Architecture

### Two-Level Implementation

**SOP Implementation (AND-OR):**

- Level 1 : Variable complements (if needed)
- Level 2 : AND gates for product terms
- Level 3 : OR gate for sum

**POS Implementation (OR-AND):**
- Level 1: Variable complements (if needed)
- Level 2: OR gates for sum terms
- Level 3: AND gate for product

### Example: SOP Implementation

For $$F = A'BC + ABC' + AC$$:

```
Inputs: A, B, C
   ↓
Level 1: NOT gates (A' from A)
   ↓
Level 2: AND gates
   - Gate 1: A'BC
   - Gate 2: ABC'
   - Gate 3: AC
   ↓
Level 3: OR gate combining all products
   ↓
Output: F
```

## Design Example: Prime Number Detector

**Problem:** Design a circuit to detect single-digit prime numbers (2, 3, 5, 7) in BCD.

**Step 1:** Truth Table (4-bit BCD input)

| Decimal | A B C D | Prime? |
|---------|---------|---------|
| 0 | 0 0 0 0 | 0 |
| 1 | 0 0 0 1 | 0 |
| 2 | 0 0 1 0 | 1 |
| 3 | 0 0 1 1 | 1 |
| 4 | 0 1 0 0 | 0 |
| 5 | 0 1 0 1 | 1 |
| 6 | 0 1 1 0 | 0 |
| 7 | 0 1 1 1 | 1 |
| 8 | 1 0 0 0 | 0 |
| 9 | 1 0 0 1 | 0 |
| 10-15 | - - - - | X (don't care) |

**Step 2:** K-Map Simplification

```
   \  CD
AB  \ 00  01  11  10
   00  0   0   1   1
   01  0   1   1   0
   11  X   X   X   X
   10  0   0   X   X
```

**Step 3:** Simplified Expression

$$F = A'C + B'CD + A'BD$$

## Historical Note

>**George Boole (November 1815 – December 1864)** was an English mathematician, educator, philosopher and logician. He worked in the fields of differential equations and algebraic logic, and is best known as the author of "An Investigation of the Laws of Thought" (1854) which contains Boolean algebra. Boolean logic is credited with laying the foundations for the Information Age.

## Key Takeaways

1. **Multiple Representations:** Logic functions can be represented as truth tables, Boolean expressions, K-maps, or circuit diagrams

2. **Canonical Forms:** Provide unique algebraic signatures for Boolean functions

3. **Simplification:** K-maps offer visual method for minimizing logic expressions

4. **Duality:** SOP and POS forms are dual representations of the same function

5. **Implementation:** Standard forms lead directly to two-level gate implementations

## Common Pitfalls to Avoid

1. **Forgetting Gray code ordering** in K-maps

2. **Missing wrap-around groups** in K-maps

3. **Not making groups as large as possible**

4. **Confusing minterms (products) with maxterms (sums)**

5. **Forgetting to include all variables** in canonical forms

## Practice Strategy

1. Start with small examples (2-3 variables)

2. Always verify simplifications with original truth table

3. Practice converting between different representations

4. Use systematic approach for K-map grouping

5. Check work using Boolean algebra laws
