# SEL-787 Transformer Protection Relay — Reference

> Condensed engineering reference generated from the SEL-787 Instruction Manual
> (`787_IM_20260130.pdf`, PM787-01, date code 20260130, 640 pages).
> Manufacturer: Schweitzer Engineering Laboratories (SEL). For authoritative
> detail always consult the full manual and the latest SEL-787 Model Option
> Table at selinc.com.

---

## 1. Overview

The **SEL-787** provides current **differential (87)** and **overcurrent**
protection for **two-winding transformers**, buses, generators, reactors, etc.
- Base relay: current differential + instantaneous and inverse time-overcurrent.
- Options: voltage-based protection, RTD-based protection, neutral current
  protection (incl. Restricted Earth Fault), extra I/O, and communications.
- All models provide monitoring/metering functions.

### Application scenarios
- Two-winding transformer differential + overcurrent + through-fault monitor.
- Above **plus** optional REF and neutral overcurrent.
- Above **plus** optional voltage, power, and frequency elements.
- Above **plus** optional internal (10 RTD card) or external (SEL-2600) RTD inputs.

---

## 2. Protection & Monitoring Features

### Standard protection
| Function | ANSI |
|---|---|
| Phase Instantaneous Overcurrent | 50P |
| Ground (Residual) Instantaneous Overcurrent | 50G |
| Negative-Sequence Instantaneous Overcurrent | 50Q |
| Phase Time Overcurrent | 51P |
| Ground (Residual) Time Overcurrent | 51G |
| Negative-Sequence Time Overcurrent | 51Q |
| Current Differential | 87 |
| Breaker Failure Protection | 50BF |

### Optional protection
- **Voltage-based** (requires AC voltage inputs option): Undervoltage (27),
  Overvoltage (59), Negative-Sequence Overvoltage (59Q), Directional Power (32),
  Loss of Potential (60LOP), Frequency (81), Volts/Hertz (24).
- **Neutral current-based**: Neutral Instantaneous OC (50N), Neutral Time OC
  (51N), Restricted Earth Fault (REF).
- **RTD-based**: up to 10 RTDs (internal 10 RTD card) or up to 12 RTDs (external
  SEL-2600 with ST option). Separate Trip and Warn settings per RTD.

### Monitoring
- Event summaries (ID, date/time, trip cause, current/voltage magnitudes).
- Event reports (filtered and raw analog data).
- Sequential Events Record (SER).
- Transformer Through-Fault Event Monitor.
- Load Profile Report.
- Full metering suite. SEL-3010 Event Messenger compatible.

---

## 3. Hardware / Models

### Card slots (six rear-panel slots)
| Slot | Function | Software ref (example) |
|---|---|---|
| **A** | Power supply + I/O (2 DI, 3 DO) — base | 1 (e.g., OUT101, IN101) |
| **B** | CPU / communications card — base | — |
| **C** | I/O expansion (comm. or digital/analog) | 3 (e.g., IN301) |
| **D** | I/O expansion or RTD card | 4 (e.g., OUT401) |
| **E** | I/O expansion or voltage/current card | 5 (e.g., AI501) |
| **Z** | 6 AC current inputs (6 ACI) — base | — |

Slots A, B, Z are base-unit slots; C, D, E are for optional cards.

### Power supply options (Slot A)
- High Voltage: 110–250 Vdc, 110–240 Vac, 50/60 Hz.
- Low Voltage: 24–48 Vdc.
- Digital input voltage options: 24, 48, 110, 125, 220, 250 Vdc/Vac.
- Card contains: 2 DI, 3 DO (two Form A normally-open, one Form C).

### Optional expansion cards
- `3 DI/4 DO/1 AO` (one per relay)
- `4 DI/4 DO`
- `4 DI/3 DO` (2 Form C, 1 Form B) — firmware R104+/R203+
- `8 DI`
- `4 AI/4 AO` (one per relay)
- `10 RTD`
- `1 ACI/3 AVI` (1 current + 3 voltage) or `1 ACI` (current) — voltage option
- `6 ACI` (Slot Z, base)
- Comm cards: `EIA-232/485`, `DeviceNet` (discontinued 2017-09-25)

### Front panel
- Large LCD display.
- 4 programmable pushbuttons with 8 LEDs.
- 8 target LEDs (6 programmable).
- Operator control interface.
- Front EIA-232 serial port (Port F).

---

## 4. Communications

### Ports
| Port | Location | Feature | Description |
|---|---|---|---|
| **F** | Front | Standard | Nonisolated EIA-232 serial |
| **1** | Rear | Optional | Isolated 10/100BASE-T copper (single/dual) or 100BASE-FX fiber Ethernet |
| **2** | Rear | Standard | Isolated multimode fiber-optic serial (ST connectors, SEL-2812 compatible) |
| **3** | Rear | Standard | Nonisolated EIA-232 or isolated EIA-485 serial |

- **IRIG-B** time-code input via B01/B02 (most models), Port 2 fiber, or Port 3
  EIA-232 (only one input at a time).
- Port F protocols: SELBOOT, Modbus RTU Slave, SEL ASCII / Compressed ASCII,
  SEL Settings File Transfer, Event Messenger.

### Protocols
- Modbus RTU slave, Modbus TCP/IP
- DNP3 serial and LAN/WAN
- Ethernet FTP, Telnet, SNTP
- MIRRORED BITS
- IEC 61850 (option)
- DeviceNet (discontinued)
- Synchrophasors with IEEE **C37.118**
- SEL ASCII, Compressed ASCII, Fast Meter, Fast Operate, Fast SER, Fast Message,
  Event Messenger

---

## 5. Differential Element (87) — Core Protection

### Characteristic
Dual-slope **percentage restraint** differential. Three differential elements
**87R-1, 87R-2, 87R-3** (one per phase pair after compensation). Each uses:
- **IOP** (Operate) = magnitude of **phasor sum** of winding currents.
- **IRT** (Restraint) = **scalar sum** of winding current magnitudes.

> NOTE: SEL-787 IRTn differs from SEL-587/SEL-387 by a factor of 2.
> For through-current at rated load: IOP ≈ 1 + (−1) = 0, IRT ≈ |1| + |−1| = 2.

Four settings define the characteristic:
- **O87P** = minimum IOP required to operate.
- **SLP1** = initial slope from origin; intersects O87P at IRT = O87P·100/SLP1.
- **IRS1** = IRT limit where SLP1 ends and SLP2 begins.
- **SLP2** = second slope (≥ SLP1).

Trip occurs when IOP > curve value for the given IRT **and** IOP > O87P.

### Element types (Relay Word bits)
- **Unrestrained** `87U1/87U2/87U3` → combined `87U`. Compare IOP to `U87P`
  (~10× TAP). No harmonic blocking/restraint. Protects bushings/end windings.
- **Restrained** `87R1/87R2/87R3` → `87R`; harmonic-restrained variants
  `87HR1/87HR2/87HR3`.
- Blocking bits: `87BL1/87BL2/87BL3`.
- `87R` and `87U` are high-speed trip elements (factory default → variable
  `TRXFMR` → asserts `TRIPXFMR` → drives OUT103 → 86 lockout).

### Data processing (per winding)
Data Acquisition → digital filters (fundamental, 2nd, 4th, 5th harmonic) →
**TAP scaling** (per-unit vs transformer MVA) → **Connection Compensation
(CTCn)**. Compensated currents: `IxWnCy` (x = phase 1–3, n = winding, y = harmonic).

### TAP scaling
Uses transformer MVA as common reference to convert secondary amps to per-unit
multiples of TAP.
- `TAP` = per-unit value common to both windings; `TAPn` = ampere value per winding.
- If **MVA ≠ OFF**: relay auto-calculates TAP1/TAP2 from MVA, winding voltage,
  CT ratio, CT connection.
- If **MVA = OFF**: enter TAP values directly.
- Constraint: max(TAPn/INOMn) / min(TAPn/INOMn) ≤ **7.5** (INOMn = 5 A or 1 A).
- **O87P** ≥ max of 0.1·INOMn/TAPn.

### Harmonics — inrush/overexcitation security
- **Even harmonics (2nd, 4th)**: security during energization/inrush.
- **5th harmonic**: security for overexcitation.
- Choose **harmonic blocking**, **harmonic restraint**, or both.
  - Blocking: 2nd/4th use **common (cross-phase) blocking** (any blocking element
    blocks all 87Rn); **5th harmonic uses independent blocking** (blocks only its
    own element).
  - Restraint: sum of 2nd+4th harmonic raises the characteristic (adds constant
    *c* to IOP = SLP1·IRT + c).
- 5th-harmonic **overexcitation alarm**: threshold `TH5P` + timer `TH5D`.

### Differential current alarm (open-CT detection)
- `87AP` (level, typically below O87P) + `87AD` (delay). Assertion of `87AT`
  indicates a differential-circuit problem (e.g., open CT). Program 87AT to act.

### Differential Element Settings (Table 4.4)
| Prompt | Range | Name := Default |
|---|---|---|
| XFMR DIFF ENABLE | Y, N | E87 := Y |
| WDG1 CURR TAP | 0.50–31.00 A (5A); 0.10–6.20 A (1A) | TAP1 := 2.09 |
| WDG2 CURR TAP | 0.50–31.00 A (5A); 0.10–6.20 A (1A) | TAP2 := 2.09 |
| OPERATE CURR LVL | 0.10–1.00 TAP | O87P := 0.30 |
| DIFF CURR AL LVL | OFF, 0.05–1.00 TAP | 87AP := 0.15 |
| DIFF CURR AL DLY | 1.0–120.0 s | 87AD := 5.0 |
| RESTRAINT SLOPE1 | 5%–90% | SLP1 := 25 |
| RESTRAINT SLOPE2 | 5%–90% | SLP2 := 70 |
| RES SLOPE1 LIMIT | 1.0–20.0 TAP | IRS1 := 6.0 |
| UNRES CURR LVL | 1.0–20.0 TAP | U87P := 10.0 |
| 2ND HARM BLOCK | OFF, 5%–100% | PCT2 := 15 |
| 4TH HARM BLOCK | OFF, 5%–100% | PCT4 := 15 |
| 5TH HARM BLOCK | OFF, 5%–100% | PCT5 := 35 |
| 5TH HARM AL LVL | OFF, 0.02–3.20 TAP | TH5P := OFF |
| 5TH HARM AL DLY | 0.0–120.0 s | TH5D := 1.0 |
| HARMONIC RESTRNT | Y, N | HRSTR := Y |
| HARMONIC BLOCK | Y, N | HBLK := N |

Typical O87P: ~0.2–0.3 for transformers, ~1.0 for buses.

---

## 6. CT / Transformer Connection Compensation (WnCTC)

Settings `W1CTC`, `W2CTC` (range **0–12**) select 3×3 matrices CTC(0)–CTC(12)
compensating phase shift in **30° increments** (0°–360°).

`[I1WnC I2WnC I3WnC]ᵀ = CTC(m) · [IAWn IBWn ICWn]ᵀ`

| WnCTC | Correction (ABC rotation) | Notes |
|---|---|---|
| 0 | 0° | Identity — no change, keeps zero-sequence |
| 1 | 30° CCW | |
| 2 | 60° CCW | |
| 3 | 90° CCW | |
| 4 | 120° CCW | |
| 5 | 150° CCW | |
| 6 | 180° CCW | |
| 7 | 210° CCW | |
| 8 | 240° CCW | |
| 9 | 270° CCW | |
| 10 | 300° CCW | |
| 11 | 330° CCW | |
| 12 | 0° (360°) | Removes zero-sequence (like all non-zero m) |

For ACB rotation the direction is CW by the same magnitude.
**CTC(0)** = identity (keeps zero-sequence). All matrices with m ≠ 0 (including
CTC(12)) **remove zero-sequence** current.

### Compensation guidelines
1. Determine phase shift as seen by the relay (needs transformer nameplate +
   3-line diagram of system/CT/relay connections).
2. Choose reference winding:
   - If a delta winding is wired in → use it as reference, set CTC(**0**).
   - If no delta winding → set CTC(**11**) for one wye winding.
3. Determine compensation for other windings relative to reference. Use **odd
   matrices** for wye windings; **avoid even matrices** when possible.

See Appendix L for worked examples.

---

## 7. Key Specifications (secondary quantities)

### Processing / oscillography
- AC sampling: **16 samples/cycle**. Frequency tracking 20–70 Hz (needs voltage option).
- Digital filter: one-cycle cosine after analog low-pass (rejects dc and all
  harmonics above fundamental).
- Protection/control processing: **4×/cycle** (math vars & analog: every 100 ms);
  51 elements: 2×/cycle.
- Oscillography length: **15 or 64 cycles**; 16 s/cy unfiltered, 4 s/cy filtered.
- SER & event time-stamp: 1 ms resolution, ±5 ms accuracy.

### Instantaneous/Definite-Time OC (50P/50G/50N/50Q)
- Pickup: 5 A models 0.50–96.00 A; 1 A models 0.10–19.20 A (0.01 A steps).
- Accuracy: ±5% of setting ±0.02·INOM. Time delay 0.00–5.00 s. Pickup/dropout <1.5 cyc.

### Inverse-Time OC (51P/51G/51N/51Q)
- Pickup: 5 A 0.50–16.00 A; 1 A 0.10–3.20 A.
- Time dial: US 0.50–15.00; IEC 0.05–1.00.
- Curves: US (U1–U5) and IEC families.

### Differential (87)
- Unrestrained pickup: 1.0–20.0 per-unit TAP.
- Restrained pickup: 0.10–1.00 per-unit TAP.
- Pickup accuracy: 5 A ±5% ±0.10 A; 1 A ±5% ±0.02 A.
- Pickup time: Unrestrained 0.8/1.0/1.9 cyc; Restrained w/ blocking 1.5/1.6/2.2 cyc;
  Restrained w/ restraint 2.62/2.72/2.86 cyc (Min/Typ/Max).
- Harmonic pickup range 5%–100% of fundamental.

### REF
- Pickup 0.05–3.00 per-unit of INOM neutral input (0.01 steps). Directional
  output 1.5 ±0.25 cyc.

### Voltage elements
- 27 / 59: Off, 12.5–300.0 V, ±1% ±0.5 V, delay 0.0–120.0 s.
- 59Q: 12.5–200.0 V, ±5% ±2 V.
- 24 (V/Hz): definite / inverse / composite / user-definable curves, pickup 100–200%.
- 32 (power): types +W, −W, +VAR, −VAR.
- 81 (frequency): Off, 20.0–70.0 Hz, ±0.01 Hz (V1>60 V), <4 cyc.

### RTD protection
- Setting Off, 1–250 °C, ±2 °C. Open-circuit >250 °C, short <−50 °C.
- Types: PT100, NI100, NI120, CU10. Lead ≤25 Ω. Update <3 s.
- RTD fault/trip/alarm delay ≈ 12 s.

### Metering accuracy (highlights)
- Phase currents ±1% ±1°. Voltages ±1% ±1° (24–264 V).
- Differential quantities ±5% ±0.1 A (5 A) / ±0.02 A (1 A).
- Real/Reactive/Apparent power ±3%. Frequency ±0.01 Hz (V1>60 V).

---

## 8. Settings Structure (SET commands)

| Command | Scope |
|---|---|
| `SET` / `SET n` | Group settings (groups 1–4) |
| `SET L` | SELOGIC control equations, variables, timers, latches, counters |
| `SET G` | Global settings (breaker failure, time/date, data reset, etc.) |
| `SET P n` | Port settings (n = 1, 2, 3, 4, F) |
| `SET F` | Front-panel settings (display points, local bits, LEDs) |
| `SET R` | Report settings (event trigger, SER, load profile) |
| `SET DNP n` | DNP3 map settings (n = 1, 2, 3) |
| `SET M` | Modbus user map settings |

Key ID settings: **RID** (Relay ID, default `SEL-787`), **TID** (Terminal ID,
default `TRNSFRMR RELAY`). Configuration/ratings: MVA, winding voltages (VWDGn),
CT ratios (CTRn), PT ratio (PTR), VNOM, DELTA_Y, WnCTC.

4 setting groups selectable via `GRO n`; copy via `COPY m n`.

---

## 9. Access Levels & Passwords

| Level | Prompt | Purpose | Default password |
|---|---|---|---|
| 0 | `=` | Minimal (ACC, ID, QUI); comm-processor support | — |
| 1 | `=>` | View info (settings, metering) — read only | **OTTER** |
| 2 | `=>>` | Change settings, control | **TAIL** |
| C (CAL) | — | SEL factory/field service only | **CLARKE** |

- Enter levels with `ACC` (→1), `2AC` (→2), `CAL` (→C).
- Change passwords: `PAS 1`, `PAS 2`, `PAS C`. Up to 12 chars, case-sensitive.
- **Change all default passwords at installation.** Commands truncate to first 3
  letters; terminate with `<Enter>` (`<CR>`).

---

## 10. Serial Command Summary (selected)

### Access Level 0
- `ACC` → Level 1 · `ID` relay ID · `QUI` → Level 0

### Access Level 1 (review)
- `2AC` → Level 2
- `CEV n` compressed event report (add `DIF` / `R`)
- `EVE n` / `EVE nR` / `EVE D n` / `EVE DIF1..3 n` event reports
- `HIS n` event history · `SER` Sequential Events Recorder report
- `MET` fundamental metering; `MET DIF` differential; `MET H` harmonics;
  `MET DEM`/`PEA` demand/peak; `MET E` energy; `MET T` RTD; `MET PM` synchrophasors;
  `MET M`/`RMS`/`MV`/`AI`
- `SHO` settings (`SHO G`, `SHO L`, `SHO F`, `SHO R`, `SHO P n`, `SHO DNP n`, `SHO M`)
- `STA` self-test status · `SUM` event summary · `TAR n` targets
- `TFE` through-fault events · `LDP` load profile · `GROUP` active group
- `DATE` / `TIME` · `IRIG` sync · `ETH`/`MAC`/`PING` Ethernet

### Access Level 2 (change / control)
- `SET ...` families (see §8), `... TERSE` to suppress auto-SHO
- `GRO n` active group · `COPY m n` copy group
- `OPE n` / `CLO n` open/close breaker n (1 or 2)
- `PUL n t` pulse output · `CON RBnn k` remote bit set/clear/pulse
- `TRI` trigger event capture · `STA C/R` clear+restart
- `PAS 1/2` change passwords · `L_D` load firmware · `LOO` MIRRORED BITS loopback
- `ANA c p t` test analog output · `DTO` download V/Hz user curve
- `FIL WRITE/READ/SHOW/DIR` file transfer

### Access Level C
- `PAS C` (SEL use only)

---

## 11. Metering & Monitoring (Section 5)

- Power measurement conventions; delta-connected CT handling.
- Small-signal cutoff for metering.
- **Load Profiling** (`LDP` / `LDP C`).
- **Through-Fault Event Monitor**: cumulative + up to 500 individual events
  (`TFE`, `TFE n`, `TFE A`, `TFE C/R`, `TFE P`).
- Harmonic report (`MET H`): 1st–5th harmonic + %THD.

---

## 12. Manual Section Map (for deep-dive)

| Section / Appendix | Topic |
|---|---|
| 1 | Introduction & Specifications |
| 2 | Installation (I/O config, connection diagrams, field serviceability) |
| 3 | PC Software (ACSELERATOR QuickSet SEL-5030) |
| 4 | Protection & Logic Functions (SET commands, SELOGIC) |
| 5 | Metering & Monitoring |
| 6 | Settings (front panel & comms), settings sheets |
| 7 | Communications (interfaces, protocols, SEL ASCII) |
| 8 | Front-Panel Operations (HMI, target LEDs) |
| 9 | Analyzing Events (event reports, SER) |
| 10 | Testing & Troubleshooting, self-test, commissioning |
| A | Firmware / ICD / Manual versions |
| B | Firmware Upgrade Instructions |
| C | SEL Communications Processors |
| D | DNP3 Communications |
| E | Modbus Communications (register map) |
| F | IEC 61850 (logical nodes, PICS, ACSI) |
| G | DeviceNet (discontinued) |
| H | MIRRORED BITS Communications |
| I | Synchrophasors (C37.118) |
| J | Relay Word Bits |
| K | Analog Quantities |
| L | Protection Application Examples (CTC & TAP examples) |

---

## 13. Quick Design Notes / Gotchas

- **TAP mismatch limit**: (TAP/INOM) ratio between windings ≤ 7.5.
- **O87P floor**: ≥ 0.1·INOMn/TAPn.
- **SLP2 ≥ SLP1** always.
- Prefer **odd CTC matrices** for wye windings; delta winding = reference CTC(0);
  no delta → reference wye = CTC(11).
- All non-identity CTC matrices remove **zero-sequence** current — important for
  external ground faults and REF coordination.
- Use **REF (87N/50N)** for improved sensitivity to ground faults near the neutral
  of a grounded-wye winding where differential is insensitive.
- Even-harmonic (2nd/4th) blocking = inrush security; 5th-harmonic = overexcitation.
- OUT103 factory default drives an **86 lockout** for high-speed 87R/87U trips.
- Only **one IRIG-B input** may be used at a time.
- Change **all default passwords** (OTTER/TAIL/CLARKE) at commissioning.
