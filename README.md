# Buffer Gate

A buffer gate **outputs the same value as its input** (`Y = A`). It does not change the logic
level, but it **cleans up and re-drives the signal** to full, solid voltage levels.

This buffer is built the simplest possible way — it is just **two NOT gates in a row**, so
there is nothing new to build at the transistor level. Each NOT gate is the complementary
`2N3904 + 2N3906` inverter board from the [`not`](https://github.com/mrmhmdalmalki/not-gate)
folder; you build that board twice and wire them together.

### Symbol

A triangle pointing in the direction of signal flow. (A NOT gate is this same triangle **plus a
bubble** on the output; a buffer is two NOT gates, so the two bubbles cancel.)

<img src="images/symbol.png" width="460">

### Truth table

| Input `A` | Output `Y` |
|:---------:|:----------:|
| 0 | 0 |
| 1 | 1 |

---

## How it is built

> buffer = NOT( NOT(A) ) = A

Two NOT gates in series. The first inverts `A` to `Ā`; the second inverts `Ā` back to `A`. Two
inversions cancel, so the output equals the input — but now re-driven to a clean, strong
`~4.8 V` for `1` and `~0.2 V` for `0`.

<img src="images/circuit.png" width="760">

Because each NOT stage is the complementary push-pull inverter, the output is **actively
driven** and can comfortably feed the next gate. That is the whole point of a buffer: restore a
tired or marginal signal back to a full, strong logic level.

---

## Building it on a breadboard

This gate has **no transistors of its own**; it is **two finished NOT-gate boards** wired in
series. Build the NOT gate twice, then connect them as shown:

<img src="images/wiring.png" width="820">

| Block | Build guide | Input | Its output goes to |
|:------|:------------|:------|:-------------------|
| **NOT board 1** | [not-gate](https://github.com/mrmhmdalmalki/not-gate) | Input `A` | the input of NOT board 2 (this node is `Ā`) |
| **NOT board 2** | [not-gate](https://github.com/mrmhmdalmalki/not-gate) | `Ā` (output of board 1) | Output `Y = A` |

Both boards share the **same +5 V rail and the same GND**. `+5 V` and `GND` are **nodes**, not
physical positions, so either rail of your breadboard can be the +5 V rail.

Quick test: Input `A` = +5 V gives Output ≈ +5 V; Input `A` = GND gives Output ≈ GND (a buffer
copies the input). The middle node `Ā` is always the opposite of both.

---

## Components

A buffer is two NOT gates, each already documented (and built from transistors) in this project:

| Block | Folder | Transistors |
|:------|:-------|:-----------:|
| NOT | [`not-gate`](https://github.com/mrmhmdalmalki/not-gate) | 1 × 2N3904 (NPN) + 1 × 2N3906 (PNP) |
| NOT | [`not-gate`](https://github.com/mrmhmdalmalki/not-gate) | 1 × 2N3904 (NPN) + 1 × 2N3906 (PNP) |

**Total: 2 × 2N3904 + 2 × 2N3906**, plus each board's base resistors (10 kΩ) and LED resistors
(220 Ω) and indicator LEDs. See the `not` folder for the exact per-board wiring.

### Power

- One shared **+5 V** rail and a common **GND** for both boards.

---

## Standards and references

**Gate symbol.** The distinctive-shape symbol follows the ANSI/IEEE standard for logic graphic symbols:

- IEEE Std 91-1984 and 91a-1991, *Graphic Symbols for Logic Functions* ([standards.ieee.org](https://standards.ieee.org/ieee/91_91a/241/)). The distinctive shapes originate from US MIL-STD-806; the international equivalent is IEC 60617-12.
- Free explainer: Texas Instruments, *Overview of IEEE Standard 91-1984* (PDF) ([ti.com](https://www.ti.com/lit/ml/sdyz001a/sdyz001a.pdf)).
- Symbols and truth tables overview: *Logic gate*, Wikipedia ([wikipedia.org](https://en.wikipedia.org/wiki/Logic_gate)).

**Transistor circuit.** Each NOT stage is a complementary (CMOS-style) push-pull inverter — a matched NPN/PNP pair sharing one output:

- *Push–pull / complementary output*, Wikipedia ([wikipedia.org](https://en.wikipedia.org/wiki/Push%E2%80%93pull_output)).
- *CMOS inverter*, Wikipedia ([wikipedia.org](https://en.wikipedia.org/wiki/CMOS#Inversion)).
- P. Horowitz and W. Hill, *The Art of Electronics*, 3rd ed., Cambridge University Press, 2015.
- A. S. Sedra and K. C. Smith, *Microelectronic Circuits*, Oxford University Press.
- T. L. Floyd, *Digital Fundamentals*, Pearson (logic-gate symbols and truth tables).

**Transistor parts.** 2N3904 NPN, onsemi datasheet ([PDF](https://www.onsemi.com/pdf/datasheet/2n3904-d.pdf)). 2N3906 PNP, onsemi datasheet ([PDF](https://www.onsemi.com/pdf/datasheet/2n3906-d.pdf)).

---

## Regenerating the diagrams

```bash
pdflatex circuit.tex
pdflatex symbol.tex
pdflatex wiring.tex
pdftoppm -png -r 400 circuit.pdf images/circuit   # -> images/circuit-1.png
pdftoppm -png -r 400 symbol.pdf  images/symbol     # -> images/symbol-1.png
pdftoppm -png -r 400 wiring.pdf  images/wiring     # -> images/wiring-1.png
```

> Use `pdftoppm`, not `pdftocairo`, at high DPI the Cairo backend can garble the fonts.
