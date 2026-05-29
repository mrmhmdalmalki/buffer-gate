# Buffer Gate

A buffer gate **outputs the same value as its input** (`Y = A`). It does not change the
logic level, but it **cleans up and re-drives the signal** to full, solid voltage levels.

### Symbol

A triangle pointing in the direction of signal flow. (A NOT gate is this same triangle
**plus a bubble** on the output.)

<img src="images/symbol.png" width="380">

### Truth table

| Input `A` | Output `Y` |
|:---------:|:----------:|
| 0 | 0 |
| 1 | 1 |

---

## What `0` and `1` really mean

`0` is **not** an empty wire. Both levels are real voltages the output is connected to:

| Level | Connected to | Voltage |
|:-----:|:-------------|:-------:|
| `1` (HIGH) | the supply rail **Vcc** | `+5 V` |
| `0` (LOW)  | **ground (GND)**          | `0 V`  |

So `0` means the output is **actively pulled down to ground (0 V)** through a conducting
transistor — not "no electricity." A wire connected to *nothing* is a third, undefined state
called **floating**; it picks up noise and reads randomly, so we never leave a node floating.

---

## How it is built

A buffer must restore full logic levels, so we chain **two NOT-gate stages**:

> buffer = NOT(NOT(A)) = A

Each stage is a **common-emitter NPN NOT gate**: emitter to ground, collector pulled up to
`+5 V` through a resistor, output taken at the collector.

<img src="images/circuit.png" width="760">

**One stage:**
- Input `1` → transistor ON → collector **pulled to ground** → stage output `0`.
- Input `0` → transistor OFF → collector **pulled to +5 V** → stage output `1`.

**Both stages together:**

| `A` | `Ā` (middle) | `Y` (output) |
|:---:|:-----------:|:------------:|
| `0` | `1` | `0` → tied to **GND** through Q2 |
| `1` | `0` | `1` → tied to **+5 V** through `R_C2` |

So `Y = A`, with a clean `0 V` for `0` and a clean `+5 V` for `1`.

---

## Components

### Transistors — 2N3904  (×2: Q1, Q2)

- **Type:** **NPN** *bipolar junction transistor* (BJT) — a current-controlled switch: a
  small current into the **base** lets a much larger current flow from **collector** to
  **emitter**. Here each transistor is used fully on/off, as a switch.
- **Package:** TO-92 (small black half-cylinder of plastic with 3 legs).
- **Pinout:** hold it with the **flat face toward you and the legs pointing down** — the pins
  are **E – B – C** (Emitter, Base, Collector) from left to right.
- **Key ratings:** V_CE ≈ **40 V** max, I_C ≈ **200 mA** max, current gain *hFE* ≈ **100–300**.
- **Why NPN (not PNP)?** The emitter sits at **ground**, so a HIGH (+5 V) on the base turns
  the transistor ON and drags its collector **down to ground**. A PNP works upside-down
  (emitter at +5 V, on when the base is LOW) and would need the circuit re-wired.
- **Substitutes:** 2N2222, PN2222, BC547 — any general-purpose NPN. **Re-check the pinout.**

### Resistors

| Ref | Value | Job |
|:---:|:-----:|:----|
| R_B1, R_B2 | **10 kΩ** | **Base resistors** — limit base current to a safe level while still switching the transistor fully on. |
| R_C1, R_C2 | **1 kΩ**  | **Collector pull-ups** — provide the HIGH (+5 V) level and limit current when the transistor pulls its output low. |

### Power

- A **+5 V** supply rail and a common **GND** (0 V) reference.

---

## Regenerating the diagrams

The schematics are drawn in LaTeX with `circuitikz`:

```bash
pdflatex circuit.tex
pdflatex symbol.tex
pdftoppm -png -r 600 circuit.pdf images/circuit   # -> images/circuit-1.png
pdftoppm -png -r 600 symbol.pdf  images/symbol     # -> images/symbol-1.png
```

> Use `pdftoppm`, not `pdftocairo` — at high DPI the Cairo backend can garble the fonts.
