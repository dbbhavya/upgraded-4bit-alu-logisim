# Upgraded 4-bit ALU – Quick Demonstration Guide

**File:** `ALU_4bit_Upgraded.circ` (Logisim-evolution 3.9.0 or newer; tested on 3.9.0 and 4.0.0)

## Operating the simulation

1. Open the file in Logisim-evolution (File → Open). The circuit **main** opens with A = 0101, B = 0011 and ADD already selected.
2. Pick the **Poke tool** (the hand icon, top-left). Simulate → Auto-Propagate should be ticked; it is on by default.
3. **A / B:** click any bit of the A or B box to toggle it.
4. **Operation:** click S2, S1, S0. The green LED under *SELECTED OPERATION* shows which operation is active.
5. **Live outputs:** you can read Y on the RESULT pin and the *LIVE Y* hex display. The live flags are the red LEDs.
6. **Store:** set LOAD = 1, then click **CLK** once. The value appears on *STORED Q* and the orange stored-flag LEDs.
7. **Hold:** set LOAD = 0 and change the inputs. Only the live values change, and *STORED Q* holds its value even if you click CLK.
8. **CLR** clears the stored result and the stored flags.
9. To see the whole circuit, use the zoom control at the bottom-left (**Auto** or **×½**).
10. Optional self-test: open Simulate → Test Vector…, load a `ALU_test_vector_*.txt` file and press Run. Every vector should pass.

## Operation codes

| S2 S1 S0 | Op | Y | Flags |
|---|---|---|---|
| 000 | ADD | A + B | CARRY, ZERO |
| 001 | SUB | A − B | BORROW (A < B), ZERO |
| 010 | AND | A AND B | ZERO |
| 011 | OR | A OR B | ZERO |
| 100 | XOR | A XOR B | ZERO |
| 101 | NOT | NOT A | ZERO |
| 110 | INC | A + 1 | CARRY (A = 1111), ZERO |
| 111 | CMP | 0001 = A>B, 0010 = A=B, 0100 = A<B | EQUAL / GREATER / LESS |

## Test table (results observed in the Logisim-evolution engine)

| TC | A | B | S2S1S0 | Op | Expected Y | Observed Y | Active flags | Result |
|---|---|---|---|---|---|---|---|---|
| 1 | 0101 | 0011 | 000 | ADD | 1000 | 1000 | none | PASS |
| 2 | 0101 | 0011 | 001 | SUB | 0010 | 0010 | none | PASS |
| 3 | 0101 | 0011 | 010 | AND | 0001 | 0001 | none | PASS |
| 4 | 0101 | 0011 | 011 | OR | 0111 | 0111 | none | PASS |
| 5 | 0101 | 0011 | 100 | XOR | 0110 | 0110 | none | PASS |
| 6 | 0101 | – | 101 | NOT | 1010 | 1010 | none | PASS |
| 7 | 0101 | – | 110 | INC | 0110 | 0110 | none | PASS |
| 8 | 0101 | 0011 | 111 | CMP | 0001 | 0001 | GREATER | PASS |
| 9 | 0011 | 0101 | 111 | CMP | 0100 | 0100 | LESS | PASS |
| 10 | 0101 | 0101 | 111 | CMP | 0010 | 0010 | EQUAL | PASS |
| 11 | 0000 | 0000 | 000 | ADD | 0000 | 0000 | ZERO | PASS |
| 12 | 1111 | 0001 | 000 | ADD | 0000 | 0000 | CARRY, ZERO | PASS |
| 13 | 0011 | 0101 | 001 | SUB | 1110 | 1110 | BORROW | PASS |

**LOAD test:** store 1000 (LOAD = 1 + CLK), then change the inputs with LOAD = 0. STORED stays at 1000, including through extra CLK pulses. The full 13-step sequence is in Section 3.5 of the report.

## 3-minute demo script

1. Show TC1 (ADD gives 1000), then store it with LOAD + CLK.
2. Set LOAD = 0 and switch to SUB with A = 0011, B = 0101. The live result is 1110 with BORROW on, while STORED still shows 8.
3. Select CMP and show GREATER, LESS and EQUAL with the three operand pairs.
4. Try 1111 + 0001 to show CARRY and ZERO together.
5. Press CLR.
