# SELOGIC 101 — Lesson (SEL-787)

> A beginner's guide to SEL's SELOGIC control-equation language, based on the
> SEL-787 Instruction Manual (PM787-01, date code 20260130).
> Includes worked examples and exercises with an answer key.

---

## Learning objectives
By the end of this lesson you should be able to:
1. Explain what SELOGIC is and how it executes.
2. Read and write basic Boolean and math SELOGIC equations.
3. Identify the operand types (Relay Word bits, SV, LT, RB, LB, MV, counters).
4. Use operators with correct precedence, including timers and edge triggers.
5. Enable logic resources and enter equations with the `SET L` command.
6. Build a small practical scheme and test it over the serial port.

---

## 1. What SELOGIC is

SELOGIC is SEL's **configurable control-equation language** — programmable
Boolean + math logic embedded in the relay's settings. You use it to build
custom trip logic, breaker control, alarms, output-contact behavior, display
logic, timers, and counters.

**Mental model:** it behaves like **PLC scan logic**, not a sequential program.
Every equation is re-evaluated continuously each processing interval (most logic
4×/cycle; math variables every 100 ms). There are **no loops or functions** — you
have fixed pools of signals/variables that you wire together with equations.

The same SELOGIC language runs across SEL's entire relay family (SEL-387,
SEL-487, SEL-700 series, etc.), so the skills transfer directly.

---

## 2. The one syntax rule that trips people up

Every equation uses the **assignment operator `:=`**, never `=`.

```
SV01 := 87R OR 51P1T
```

Why? Because `=` is reserved for the **equality comparison** operator. Both
Boolean and math equations "assign" a result with `:=`.

Comments start with `#`:

```
SV01 := 87R OR 51P1T   # any differential or phase-TOC trip
```

---

## 3. The two equation types

| Type | Works on | Result goes to | Example |
|---|---|---|---|
| **Boolean** | logical 0/1 signals (Relay Word bits) | `SVnn`, outputs, latches | `OUT101 := NOT(RB01 OR SV02)` |
| **Math** | numbers | Math variables `MVnn` | `MV01 := 12 * IN101` |

---

## 4. Operands — the signals you work with

| Name | Range | What it is |
|---|---|---|
| **Relay Word bits** | — | Element/status bits: `87R`, `50P1`, `51P1T`, `IN101`, `OUT103`, `52A1` |
| **SELOGIC variables/timers** | `SV01`–`SV32` | Your own logic flags; each has timers `SVnPU`/`SVnDO` (0–3000 s) |
| **Latch bits** | `LT01`–`LT32` | Set/reset latches; **nonvolatile** (survive power loss) |
| **Remote bits** | `RB01`–`RB32` | Controlled via serial (`CON RBnn S/C/P`) — great for testing |
| **Local bits** | `LB01`–`LB32` | Controlled from front-panel pushbuttons |
| **Math variables** | `MV01`–`MV32` | Store numerical results (±16,777,215.99) |
| **Counters** | `SC01`–`SC32` | Count events |

---

## 5. Operators (highest → lowest precedence)

| Operator | Function | Type |
|---|---|---|
| `( )` | Grouping (up to 14 sets, nestable) | Both |
| `–` | Negation | Math |
| `NOT` | Boolean NOT | Boolean |
| `R_TRIG` / `F_TRIG` | Rising / falling edge (1-interval pulse) | Boolean |
| `*` `/` | Multiply / divide | Math |
| `+` `-` | Add / subtract | Math |
| `< > <= >=` | Comparison | Boolean |
| `= <>` | Equality / inequality | Boolean |
| `AND` | AND | Boolean |
| `OR` | OR | Boolean |

- Evaluated **left-to-right within precedence**.
- Max **15 elements** per equation.
- `R_TRIG`/`F_TRIG` apply to **single bits only** (not to groups in parentheses).

---

## 6. Enable resources before using them

Only enabled logic elements appear for setting. Set the counts first (`SET L`):

| Setting | Meaning | Default |
|---|---|---|
| `ELAT` | # of SELOGIC latches | 4 |
| `ESV` | # of SV variables/timers | 5 |
| `ESC` | # of SELOGIC counters | N (none) |
| `EMV` | # of math variables | N (none) |

E.g., set `ESV := 10` to make SV01–SV10 available.
(A math variable set to `NA` is treated as 0.)

---

## 7. Where you enter it

- **`SET L`** command — SELOGIC variables, timers, latches, counters, enables.
- SELOGIC also appears in other setting groups: **trip equations** (`TR`, `TRIP`),
  **output contacts** (`OUT101 := ...`), **latch set/reset** (`SETnn`/`RSTnn`).
- Or graphically in **ACSELERATOR QuickSet (SEL-5030)** — a rules-based editor.

---

## 8. Worked examples

### Example 1 — Simple OR into a flag
```
SV01 := 87R OR 87U OR 51P1T
```
`SV01` asserts (=1) whenever a differential trip (restrained OR unrestrained)
OR a phase time-overcurrent trip is present.

### Example 2 — Drive an output contact with NOT
```
OUT101 := NOT(RB01 OR SV02)
```
`OUT101` energizes only when **both** `RB01` and `SV02` are deasserted.
(NOT of a group: `NOT(0 OR 0) = NOT(0) = 1`.)

### Example 3 — Timer (dropout delay)
```
SV03   := 51P1
SV03DO := 2.00        # SV03T holds 2 s after SV03 drops out
```
`SV03T` follows `51P1` but stays asserted 2 seconds after it clears — useful for
seal-in / anti-chatter.

### Example 4 — Latch (set/reset, survives power cycle)
```
SET05 := 87AT                 # differential CT alarm sets latch 5
RST05 := PB01 AND LT05        # front-panel pushbutton 1 acknowledges/clears it
```
`LT05` latches on a differential CT-circuit alarm and stays set (even through a
power loss) until an operator presses pushbutton 1.

### Example 5 — Edge trigger (one-shot pulse)
```
SV06 := R_TRIG 52A1           # pulses for one interval when breaker closes
```

### Example 6 — Math if/else
```
MV01 := 12 * IN101 + (MV01 + 1) * NOT IN101
```
`MV01` becomes 12 whenever `IN101` is asserted; otherwise it increments by 1
each scan. (Shows how math + NOT builds simple if/else behavior.)

### Example 7 — Conditional display + testable via serial
```
# SET L
SV10   := 87R OR 87U OR 51P1T OR 87AT OR RB01   # "fault present" (RB01 = test hook)
SET10  := SV10                                   # latch it
RST10  := PB01 AND LT10                           # acknowledge with pushbutton 1

# SET F  (Boolean display point, hidden until LT10 asserts)
DP05   := LT10,"*** FAULT — ACK PB1 ***",
```
Bench test with **no CT/PT signals**:
```
=>>CON RB01 S     # asserts fault → latches LT10 → unhides DP05
=>>CON RB01 C     # clear the test hook (latch stays until PB1 ack)
```

---

## 9. Gotchas to remember

- **Math is integer-scaled (×128, rounded)** → results are rounded. **Order
  matters**: multiply before dividing. `(60 * 100000)/4160` is far more accurate
  than `(60/4160) * 100000`.
- **Latch bits wear flash memory** — rated ~5000 cumulative state changes/day
  over 25 years. Don't toggle them rapidly.
- **No self-oscillation allowed** — set/reset equations can't create continuous
  cyclic latching; qualify with timers.
- **Everything runs every scan** — think "what should this signal equal right
  now," not step-by-step execution.
- **`:=` not `=`** for assignment.

---

## 10. Exercises

Try writing the SELOGIC before checking the answer key in Section 11.
Assume resources are enabled as needed.

**Exercise 1 (Boolean OR).**
Create `SV01` that asserts if *any* of these trips are present: restrained
differential (`87R`), unrestrained differential (`87U`), or negative-sequence
time-overcurrent (`51Q1T`).

**Exercise 2 (AND + NOT).**
Energize output `OUT102` when input `IN101` is asserted **and** input `IN102`
is **not** asserted.

**Exercise 3 (Timer).**
Make `SV05T` assert 3 seconds *after* `SV05` first asserts (pickup delay), where
`SV05 := 50P1`. Which setting name and value do you use?

**Exercise 4 (Latch).**
Latch bit `LT02` should **set** when the 5th-harmonic overexcitation alarm bit
`TH5AT` asserts, and **reset** when local bit `LB03` is on. Write `SET02` and
`RST02`.

**Exercise 5 (Edge trigger).**
Produce a one-scan pulse on `SV07` each time breaker aux contact `52A2`
transitions from open to closed.

**Exercise 6 (Parentheses precedence).**
For `OUT103 := IN101 OR IN102 AND IN103`, given `AND` has higher precedence
than `OR`: if `IN101=1, IN102=0, IN103=0`, what is `OUT103`? Then rewrite the
equation with parentheses so that the OR is evaluated first, and state the new
result for the same inputs.

**Exercise 7 (Math if/else).**
Write `MV02` so it equals 100 when `IN102` is asserted and 0 otherwise.

**Exercise 8 (Math accuracy).**
You must compute `MV03 = (25 / 3730) * 480000`. Rewrite it in the form that
minimizes rounding error, and explain why.

**Exercise 9 (Applied scheme).**
Build a "breaker-open alarm": assert `SV08` if breaker 1 has been open
(`52A1` deasserted) continuously for 5 seconds. (Hint: use a timer on the NOT of
`52A1`.)

**Exercise 10 (Testable design).**
Extend Exercise 9 so the alarm can also be forced for bench testing using remote
bit `RB05` over the serial port, and drive output `OUT104` from the result.

---

## 11. Answer key

**1.**
```
SV01 := 87R OR 87U OR 51Q1T
```

**2.**
```
OUT102 := IN101 AND NOT IN102
```

**3.** Use the pickup-delay timer setting for SV05:
```
SV05   := 50P1
SV05PU := 3.00     # SV05T asserts 3 s after SV05 picks up
```

**4.**
```
SET02 := TH5AT
RST02 := LB03
```

**5.**
```
SV07 := R_TRIG 52A2
```

**6.**
- As written, `AND` binds first: `IN102 AND IN103 = 0 AND 0 = 0`, then
  `IN101 OR 0 = 1 OR 0 = 1`. So **`OUT103 = 1`**.
- Rewritten to force OR first:
  ```
  OUT103 := (IN101 OR IN102) AND IN103
  ```
  For the same inputs: `(1 OR 0) AND 0 = 1 AND 0 = 0`. So **new result = 0**.

**7.**
```
MV02 := 100 * IN102
```
(`IN102` evaluates to 1 or 0, so the product is 100 or 0 — a clean if/else.)

**8.**
```
MV03 := (25 * 480000) / 3730
```
Multiply before dividing so the small numerator (25) isn't divided by the larger
3730 first (which would round to a coarse value and then be amplified by the
480000 multiply). Doing the large multiply first preserves precision through the
×128 integer scaling.

**9.**
```
SV08   := NOT 52A1     # true while breaker 1 is open
SV08PU := 5.00         # SV08T asserts after 5 s of continuous open
```
(Use `SV08T` as the alarm signal.)

**10.**
```
SV08   := NOT 52A1 OR RB05     # RB05 = serial test hook
SV08PU := 5.00
OUT104 := SV08T
```
Bench test with no primary signals:
```
=>>CON RB05 S     # after 5 s, SV08T asserts and OUT104 energizes
=>>CON RB05 C     # release the test hook
```

---

## 12. Where to go next
- **Manual Section 4** — Logic Settings (`SET L`), trip/close logic, full operator
  and Relay Word bit tables.
- **Appendix J** — complete Relay Word bit list (all valid operand names).
- **ACSELERATOR QuickSet (SEL-5030)** — graphical rules-based SELOGIC editor.
- Practice: type Exercise 10's scheme into a spare relay (or QuickSet) and drive
  it with `CON RB05` — validates logic with zero CT/PT signals.
