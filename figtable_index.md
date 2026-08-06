# SEL-787 Figure & Table Index

Quick-lookup index mapping each figure/table to its **PDF page** (physical 1-based page in `787_IM_20260130.pdf`) for fast rendering & analysis.

**To render a page to an image for visual analysis (0-indexed in code, so subtract 1):**

```powershell
python -c "import fitz; d=fitz.open(''787_IM_20260130.pdf''); d[PDFPAGE-1].get_pixmap(dpi=200).save(''fig.png'')"
```

Requires PyMuPDF (`pip install pymupdf`). Note: a figure may render one page off if the caption sits at a page boundary; check PDFPAGE and PDFPAGE-1/+1.

## Figures

| Figure | PDF Page | Caption |
|---|---|---|
| 1.1 | 32 | Response Header |
| 1.2 | 32 | STA Command Response—No DeviceNet Communications Card or EIA-232/EIA-485 |
| 1.3 | 33 | STA Command Response—With DeviceNet Communications Card |
| 2.1 | 42 | Relay Panel-Mount Dimensions |
| 2.2 | 43 | Slot Allocations for Different Cards |
| 2.3 | 53 | Circuit Board of Analog I/O Board, S howing Jumper Selection |
| 2.4 | 54 | JMP1 Through JMP4 Locations on 4 AI/4 AO Board |
| 2.5 | 54 | Current Output Jumpers |
| 2.6 |  | V oltage Output Jumpers |
| 2.7 | 55 | Pins for Password Jumper, Breaker Control Jumper, and SEL |
| 2.8 | 57 | Rear-Panel Connections of Select ed Cards |
| 2.9 | 58 | Dual-Fiber, Ethernet, EIA-232 Communication, 3 DI/4 DO/1 AO, |
| 2.10 | 58 | Single Copper Ethernet, 8 DI, RTD, and 4 AI/4 AO Option |
| 2.11 | 59 | DeviceNet, Fast Hybrid 4 DI/4 DO, and Current/V oltage Option |
| 2.12 | 60 | Control I/O Connections—4 AI/4 AO Option in Slot D and Fiber-Optic Port in Slot B |
| 2.13 | 61 | Control I/O Connections—Internal RTD Option |
| 2.14 | 62 | Analog Output Wiring Example |
| 2.15 | 62 | Output OUT103 Relay Output Contact Configuration |
| 2.16 | 62 | Breaker Trip Coil Connection With OUT103FS := Y and OUT103FS := N |
| 2.17 | 64 | Single-Phase V oltage Connections |
| 2.18 | 65 | V oltage Connections |
| 2.19 | 66 | Typical Current Connections |
| 2.20 | 67 | SEL-787 Provides Basic Two-Winding Transformer Differential Protection |
| 2.21 | 68 | SEL-787 Provides Two-Winding Transformer Differential Protection |
| 2.22 | 66 | SEL-787 Provides Two-Winding Transformer Differential Protection |
| 2.23 | 70 | SEL-787 Provides Autotransformer Differen tial Protection, Including REF Protection |
| 2.24 | 71 | Example DC Connections |
| 3.1 | 74 | Serial Port Communication Dial og Box |
| 3.2 | 74 | Serial Port Communication Paramete rs Dialog Box |
| 3.3 | 75 | Network Communication Parameters Di alog Box |
| 3.4 | 76 | Tools Menu |
| 3.5 | 76 | Device Response to the ID Command |
| 3.6 | 80 | Selection of Drivers |
| 3.7 | 80 | Update Part Number |
| 3.8 | 80 | New Setting Screen |
| 3.9 | 82 | Expressions Created With Expression Builder |
| 3.10 | 83 | Retrieve Events Screen |
| 3.11 | 84 | Saving the Retrieved Event |
| 3.12 | 84 | Device Overview Screen |
| 3.13 | 86 | Control Screen |
| 3.14 | 86 | Remote Operation Selection |
| 4.1 | 94 | Percentage Restraint Differential Characteristic |
| 4.2 | 94 | Winding 1 Compensated Currents |
| 4.3 | 95 | Differential Element (87-1) Quantities |
| 4.4 | 96 | Differential Element Decision Logic |
| 4.5 | 98 | Differential Element Harmonic Bloc king Logic |
| 4.6 | 99 | Differential Current Alarm Logi c Diagram |
| 4.7 | 107 | Primary Currents and Secondary Currents as Measured by the Relay |
| 4.8 | 111 | REF Directional Element |
| 4.9 | 111 | REF1 Enable Logic |
| 4.10 | 112 | REF1 Directional Element |
| 4.11 | 112 | REF Element Trip Output |
| 4.12 | 113 | Internal Fault With LV Breaker Open |
| 4.13 | 114 | REF Protection Output (Extremely Inverse-Time O/C) |
| 4.14 | 115 | Single-Wye Winding REF Application (REF1POL := 2) |
| 4.15 | 115 | Autotransformer REF Application (R EF1POL := 12) |
| 4.16 | 116 | Instantaneous Overcurrent Element Logic |
| 4.17 | 119 | Maximum Phase Time-Overcurrent El ements 51P1T and 51P2T |
| 4.18 | 120 | Phase A, B, and C Time-Overcurrent Elements |
| 4.19 | 122 | Residual Time-Overcurrent Elements 51G1T and 51G2T |
| 4.20 | 123 | Negative-Sequence Time-Overcurrent Element 51Q1T and 51Q2T |
| 4.21 | 124 | Neutral Time-Overcurrent Elements 51N1T and 51N2T |
| 4.22 | 124 | U.S. Moderately Inverse Curve: U1 |
| 4.23 | 124 | U.S. Inverse Curve: U2 |
| 4.24 | 124 | U.S. Very Inverse Curve: U3 |
| 4.25 | 125 | U.S. Extremely Inverse Curve: U4 |
| 4.26 | 125 | U.S. Short-Time Inverse Curve: U5 |
| 4.27 | 125 | IEC Class A Curve (Standard Inverse): C1 |
| 4.28 | 125 | IEC Class B Curve (Very Inverse): C2 |
| 4.29 | 125 | IEC Class C Curve (Extremely Inverse): C3 |
| 4.30 | 125 | IEC Long-Time Inverse Curve: C4 |
| 4.31 | 118 | IEC Short-Time Inverse Curve: C5 |
| 4.32 | 132 | Undervoltage Element Logic |
| 4.33 | 132 | Overvoltage Element Logic |
| 4.34 | 133 | V/Hz Element Logic |
| 4.35 | 134 | Dual-Level V olts/Hertz Time-Delay Characteristic, 24CCS = DD |
| 4.36 | 134 | Composite Inverse/Definite-Time Overexcitation Characteristic, 24CCS = ID |
| 4.37 | 137 | V olts/Hertz Inverse-Time Characteristic, 24IC = 0.5 |
| 4.38 | 138 | V olts/Hertz Inverse-Time Characteristic, 24IC = 1 |
| 4.39 | 139 | V olts/Hertz Inverse-Time Characteristic, 24IC = 2 |
| 4.40 | 140 | Three-Phase Power Elements Logic |
| 4.41 | 140 | Power Elements Oper ation in the Real/Reactive Power Plane |
| 4.42 | 142 | Over- and Underfrequency Element Logic |
| 4.43 | 144 | Loss-of-Potential (LOP) Logi c |
| 4.44 | 145 | Demand Current Logic Outputs |
| 4.45 | 146 | Response of Thermal and Rolling Demand Meters to a Step Input |
| 4.46 | 147 | V oltage V |
| 4.47 | 150 | Trip Logic |
| 4.48 | 151 | Close Logic |
| 4.49 | 153 | Schematic Diagram of a Traditional Latching Device |
| 4.50 | 153 | Logic Diagram of a Latch Switch |
| 4.51 | 155 | SEL |
| 4.52 | 158 | Result of Falling-Edge Operator on a Deasserting Input |
| 4.53 | 159 | Example Use of SEL OGIC Variables/Timers |
| 4.54 | 160 | Counter 01 |
| 4.55 | 161 | Example of the Effects of the Input Precedence |
| 4.56 | 164 | Phase Rotation Setting |
| 4.57 | 169 | Breaker Failure Logic |
| 4.58 | 170 | Analog Input Card Adaptive Name |
| 4.59 | 172 | Settings to Configure Input 1 as a 4–20 mA Transducer Measuring Temperatures |
| 4.60 | 173 | Analog Output Number Allocation |
| 4.61 | 174 | Analog Output Settings |
| 4.62 | 175 | DC Mode Processing |
| 4.63 | 175 | AC Mode Processing |
| 4.64 | 175 | Timing Diagram for Debounce Timer Opera tion When Operating in AC Mode |
| 4.65 | 184 | Display Point Settings |
| 4.66 | 184 | Front-Panel Display— Both HV and LV Breakers Open |
| 4.67 | 185 | Front-Panel Display— HV Breaker Closed, LV Breaker Open |
| 4.68 | 185 | Front-Panel Display— Both HV and LV Breakers Closed |
| 4.69 | 185 | Front-Panel Display—HV Breaker Open , LV Breaker Closed |
| 4.70 | 185 | Front-Panel Display—HV Breaker Open , LV Breaker Closed |
| 4.71 | 186 | Front-Panel Display for a Binary Entry in the Name String Only |
| 4.72 | 186 | Front-Panel Display fo r an Analog Entry in the Name String Only |
| 4.73 | 187 | Front-Panel Display for an Entry in (a) Boolean Name and Alias Strings and |
| 4.74 | 187 | Front-Panel Display for an Entry in (a) Boolean Name and Alias Strings and |
| 4.75 | 188 | Adding Temperature Measurement Display Points |
| 4.76 | 189 | Rotating Display |
| 4.77 | 190 | Adding Two Local Bits |
| 5.1 | 198 | Complex Power Measurement Conventions |
| 5.2 | 200 | METER Command Report With V oltage Option |
| 5.3 | 201 | METER DIF (Differential) Command Report |
| 5.4 | 202 | METER T Command Report With RTDs |
| 5.5 | 202 | METER E Command Report |
| 5.6 | 202 | METER RE Command Report |
| 5.7 | 204 | METER M Command Report |
| 5.8 | 204 | METER RM Command Response |
| 5.9 | 204 | MET MV Command Report |
| 5.10 | 205 | METER RMS Command Report |
| 5.11 | 206 | METER AI Command Report |
| 5.12 | 206 | METER DEM Command Report |
| 5.13 | 207 | METER P Command Report |
| 5.14 | 207 | METER H Command Report |
| 5.15 | 209 | LDP Command Report |
| 5.16 | 209 | Transformer Bank Subjected to Through Fault |
| 5.17 | 209 | Category IV Transformers Through-F ault Protection Curves |
| 5.18 | 211 | Through-Fault Diagram |
| 5.19 | 213 | Result of the TFE Command |
| 5.20 | 214 | Preload the Values of the Accumulated Data |
| 6.1 | 216 | Front-Panel Setting Entry Example |
| 7.1 | 289 | Simple Ethernet Network Configuration |
| 7.2 | 289 | Ethernet Network Configuration With Dual Redundant Connections (Failover Mode) |
| 7.3 | 290 | Ethernet Network Configuration With Ring Structure (Switched Mode) |
| 7.4 | 291 | IRIG-B Input (Relay Terminals B01–B02) |
| 7.5 | 292 | IRIG-B Input Via EIA-232 Port 3 (SEL Communications Processor as Source) |
| 7.6 | 292 | IRIG-B Input Via EIA-232 Port 3 (SEL-2401/2407/2488 Time Source) |
| 7.7 | 292 | IRIG-B Input Via Fiber-Optic EIA-232 Po rt 2 (SEL-2030/2032 Time Source) |
| 7.8 | 293 | IRIG-B Input Via Fiber-Optic EIA-232 Po rt 2 (SEL-2401/2407/2488 Time Source) |
| 7.9 | 293 | EIA-232 DB-9 Connector Pin Numbers |
| 7.10 | 294 | SEL Cable C234A—SEL-787 to DTE Device |
| 7.11 | 294 | SEL Cable C227A—SEL-787 to DTE Device |
| 7.12 | 294 | SEL Cable C222—SEL-787 to Modem |
| 7.13 | 295 | SEL Cable C272A—SEL-787 to SEL Communications Processor |
| 7.14 | 295 | SEL Cable C273A—SEL-787 to SEL Communications Processor |
| 7.15 | 295 | SEL Cable C387—SEL-787 to SEL-3010 |
| 7.16 | 310 | Ethernet Port (PORT 1) Status Report |
| 7.17 | 310 | Non-Redundant Port Response |
| 7.18 | 313 | GOOSE Command Response |
| 7.19 | 320 | PING Command Response |
| 7.20 | 324 | SHOW Command Example |
| 7.21 | 327 | Typical Relay Output for STATUS S Command |
| 8.1 | 328 | Front-Panel Overview |
| 8.2 | 333 | Access Level Security Padlock Symbol |
| 8.3 | 334 | Password Entry Screen |
| 8.4 |  | Front-Panel Pushbuttons |
| 8.5 | 335 | Main Menu |
| 8.6 | 336 | MAIN Menu and METER Submenu |
| 8.7 | 336 | METER Menu and ENERGY Submenu |
| 8.8 | 336 | Relay Response When Demand, Peak Demand , Energy, or Max/Min Metering Is Reset |
| 8.9 | 336 | Relay Response When No Analog Cards Are Installed |
| 8.10 | 337 | Relay Response When No Math Variables Enabled |
| 8.11 | 337 | MAIN Menu and EVENTS Submenu |
| 8.12 | 337 | EVENTS Menu and DISPLAY Submenu |
| 8.13 | 337 | Relay Response When No Event Data Available |
| 8.14 | 337 | Relay Response When Events Are Cleared |
| 8.15 | 338 | MAIN Menu and TARGETS Submenu |
| 8.16 | 338 | TARGETS Menu Navigation |
| 8.17 | 338 | MAIN Menu and CONTROL Submenu |
| 8.18 | 339 | CONTROL Menu and OUTPUTS Submenu |
| 8.19 | 339 | CONTROL Menu and LOCAL BITS Submenu |
| 8.20 | 340 | MAIN Menu and SET/SHOW Submenu |
| 8.21 | 341 | SET/SHOW Menu |
| 8.22 | 342 | MAIN Menu and Status Submenu |
| 8.23 | 342 | Factory-Default Front-Panel LEDs |
| 8.24 | 343 | Target Reset Pushbutton |
| 8.25 | 190 | Operator Control Pushbuttons and LEDs |
| 9.1 | 349 | Example Event Summary |
| 9.2 | 352 | Sample Event History |
| 9.3 | 354 | Example Standard 15-Cycle Analog Even t Report 1/4-Cycle Resolution |
| 9.4 | 358 | Derivation of Analog Event Report Current Values and RMS Current Values From |
| 9.5 | 359 | Derivation of Phasor RMS Current Values From Event Report Current V alues |
| 9.6 | 364 | Example Standard 15-cycle Dig ital Event Report (EVE D Command) |
| 9.7 | 367 | Example Standard 15-cycle Differe ntial Event Report (EVE DIF1 Command) |
| 9.8 | 369 | Example Sequential Events Recorder (SER) Event Report |
| 10.1 | 372 | Low-Level Test Interface (J2 and J3) |
| 10.2 | 375 | Three-Phase Wye AC Connections |
| 10.3 | 375 | Three-Phase Open-Delta AC Connections |
| 10.4 | 377 | CTR1 Current Source Connections |
| 10.5 | 378 | CTR2 Current Source Connections |
| 10.6 | 378 | Wye V oltage Source Connections |
| 10.7 | 379 | Delta V oltage Source Connections |

## Tables

| Table | PDF Page | Caption |
|---|---|---|
| 1.1 | 31 | SEL-787 Serial Port Settings |
| 2.1 | 44 | Power Supply Inputs (PSIO/2 DI/3 DO) Ca rd Terminal Designations |
| 2.2 | 44 | Communications Ports |
| 2.3 | 45 | Communication Card Interfaces and Connectors |
| 2.4 | 46 | 6 ACI Current Inputs Card Terminal Designations |
| 2.5 | 47 | 1 ACI/3 A VI or 1 ACI Current/V oltage Inputs Card Terminal Designations |
| 2.6 | 47 | Four Analog Inputs/Four An alog Outputs (4 AI/4 AO) Card Terminal Designations |
| 2.7 | 48 | I/O (3 DI/4 DO/1 AO) Card Terminal Designations |
| 2.8 | 48 | RTD (10 RTD) Card Terminal Designations |
| 2.9 | 49 | Four Digital Inputs/Four Digital Outputs (4 DI/4 DO) Card Terminal Designations |
| 2.10 | 49 | Eight Digital Inputs (8 DI) Card Terminal Designations |
| 2.11 | 50 | Four Digital Inputs, One Form B Digital Output, Two Form C |
| 2.12 | 56 | Jumper Functions and Default Positions |
| 3.1 | 73 | SEL Software Solutions |
| 3.2 | 73 | ACSELERATOR QuickSet SEL-5030 Software |
| 3.3 | 79 | File/Tools Menus |
| 3.4 | 87 | QuickSet Help |
| 4.1 | 90 | Identifier Settings |
| 4.2 | 91 | Configurations and Ratings (Phase CTs, Power Transformer) |
| 4.3 | 92 | Configurations and Ratings (Optional Neutral CT, Phase PT) |
| 4.4 | 98 | Differential Element Settings |
| 4.5 | 102 | WnCTC Setting: Corresponding Phase and Direction of Correction |
| 4.6 | 111 | Restricted Earth Fault Settings |
| 4.7 | 116 | Winding n Maximum Phase Overcurrent Settings (n = 1 or 2) |
| 4.8 | 118 | Winding n Residual Overcurrent Settings (n = 1, 2) |
| 4.9 | 118 | Winding n Negative-Sequence Overcurrent Settings (n = 1 or 2) |
| 4.10 | 119 | Winding n Maximum Phase Time-Overc urrent (n = 1 or 2) |
| 4.11 | 120 | Winding n Phase A, B, and C Time-Overcurrent (n = 1 or 2) |
| 4.12 | 121 | Residual Time-Overcurrent Settings (n = 1 or 2) |
| 4.13 | 122 | Winding n Negative-Sequence Time-Overcurrent Settings (n = 1, 2) |
| 4.14 | 123 | Neutral Overcurrent Settings |
| 4.15 | 123 | Neutral Time-Overcurrent Settings |
| 4.16 | 125 | Equations Associated With U.S. Curves |
| 4.17 | 124 | Equations Associated With IEC Curves |
| 4.18 | 128 | RTD Settings |
| 4.19 | 130 | RTD Resistance Versus Temperature |
| 4.20 | 130 | Undervoltage Settings |
| 4.21 | 131 | Overvoltage Settings |
| 4.22 | 135 | V olts Per Hertz Settings |
| 4.23 | 140 | Power Element Settings |
| 4.24 | 142 | Frequency Settings |
| 4.25 | 144 | Demand Meter Settings |
| 4.26 | 149 | Trip/Close Logic Settings |
| 4.27 | 153 | Enable Settings |
| 4.28 | 154 | Latch Bits Equation Settings |
| 4.29 | 156 | SEL |
| 4.30 | 159 | Other SEL OGIC Control Equation Operators/V alues |
| 4.31 | 160 | SEL OGIC Variable Settings |
| 4.32 | 161 | Counter Input/Output Description |
| 4.33 | 161 | Order of Precedence of the Control Inputs |
| 4.34 | 162 | Control Output Equations and Contact Behavior Settings |
| 4.35 | 136 | General Global Settings |
| 4.36 | 166 | Setting Group Selection |
| 4.37 | 167 | Time and Date Management Settings |
| 4.38 | 169 | Breaker Failure Setting |
| 4.39 | 171 | Summary of Steps |
| 4.40 | 172 | Analog Input Card in Slot 3 |
| 4.41 | 173 | Output Setting for a Card in Slot 3 |
| 4.42 | 176 | Slot C Input Debounce Settings |
| 4.43 | 176 | Data Reset Setting |
| 4.44 | 177 | Setting Change Disable Setting |
| 4.45 | 177 | Time-Synchronization Source Setting |
| 4.46 | 178 | Front-Panel Serial Port Se ttings |
| 4.47 | 178 | Ethernet Port Settings |
| 4.48 | 179 | Port Number Settings That Must be Unique |
| 4.49 | 179 | Fiber-Optic Serial Port Settings |
| 4.50 | 180 | Rear-Panel Serial Port Settings |
| 4.51 | 177 | Rear-Panel Serial Port (EIA-232/E IA-485) Settings |
| 4.52 | 181 | Rear-Panel DeviceNet Port Settings |
| 4.53 | 181 | Display Point and Local Bit Default Settings |
| 4.54 | 182 | Front-Panel General Settings |
| 4.55 | 182 | LCD Display Point Settings |
| 4.56 | 183 | Settings That Always, Never, or Conditionally Hide a Display Point |
| 4.57 | 184 | Entries for the Four Strings |
| 4.58 | 185 | Binary Entry in the Name String Only |
| 4.59 | 186 | Analog Entry in the Name String Only |
| 4.60 | 187 | Entry in the Name String and the Alias Strings |
| 4.61 | 188 | Example Settings and Displays |
| 4.62 | 191 | Target LED Settings |
| 4.63 | 191 | Pushbutton LED Settings |
| 4.64 | 192 | Auto-Removal Settings |
| 4.65 | 192 | SER Trigger Settings |
| 4.66 | 193 | Enable Alias Settings |
| 4.67 | 193 | SET R SER Alias Settings |
| 4.68 | 194 | Event Report Settings |
| 4.69 | 194 | Load Profile Settings |
| 4.70 | 194 | DNP Map Settings |
| 4.71 | 195 | User Map Register Settings |
| 5.1 | 199 | Measured Fundamental Meter Values |
| 5.2 | 201 | Measured Differential Meter Values |
| 5.3 | 201 | Thermal Meter Values |
| 5.4 | 201 | RTD Input Status Messages |
| 5.5 | 203 | Maximum/Minimum Meter Values |
| 5.6 | 205 | RMS Meter Values |
| 5.7 | 206 | Demand Values |
| 5.8 | 207 | Measured Harmonic Meter Values |
| 5.9 | 208 | Synchrophasor Measured Values |
| 5.10 | 211 | Through-Fault Element Settings |
| 5.11 | 214 | Through-Fault Events Report Messages |
| 6.1 | 215 | Methods of Accessing Settings |
| 6.2 | 218 | SHOW Command Options |
| 6.3 | 218 | SET Command Options |
| 6.4 | 219 | SET Command Editing Keystrokes |
| 6.5 | 219 | SET Command Format |
| 6.6 | 220 | Setting Interdependency Error Messages |
| 7.1 | 287 | SEL-787 Communications Port Interfaces |
| 7.2 | 293 | EIA-232/EIA-485 Serial Port Pin Functions |
| 7.3 | 37 | Protocols Supported on the Various Ports |
| 7.4 | 299 | Settings Associated With SNTP |
| 7.5 | 301 | Serial Port Automatic Messages |
| 7.6 | 303 | Command Response Header Definitions |
| 7.7 | 304 | ACCESS Commands |
| 7.8 | 306 | ANALOG Command |
| 7.9 | 308 | COM Command |
| 7.10 | 309 | CONTROL Command |
| 7.11 | 309 | COPY Command |
| 7.12 | 309 | COUNTER Command |
| 7.13 | 310 | DATE Command |
| 7.14 | 311 | EVENT Command (Event Reports) |
| 7.15 | 311 | FILE Command |
| 7.16 | 312 | GOOSE Command Variants |
| 7.17 | 313 | GROUP Command |
| 7.18 | 314 | HELP Command |
| 7.19 | 314 | HISTORY Command |
| 7.20 |  | IDENTIFICATION Command |
| 7.21 | 315 | IRIG Command |
| 7.22 | 315 | L_D Command (Load Firmware) |
| 7.23 | 316 | LDP Commands |
| 7.24 | 316 | LOO Command |
| 7.25 | 317 | METER Command |
| 7.26 | 317 | Meter Class |
| 7.27 | 319 | PASSWORD Command |
| 7.28 | 319 | Factory-Default Passwords for Access Levels 1, 2, and C |
| 7.29 | 319 | Valid Password Characters |
| 7.30 | 321 | PUL OUT nnn Command |
| 7.31 | 321 | QUIT Command |
| 7.32 | 321 | R_S Command (Restore Factory Defaults) |
| 7.33 | 322 | SER Command (Sequential Events R ecorder Report) |
| 7.34 | 322 | SER D Command |
| 7.35 | 323 | SET Command (Change Settings) |
| 7.36 | 323 | SET Command Editing Keystrokes |
| 7.37 | 324 | SHOW Command (Show/View Settings) |
| 7.38 | 326 | STATUS Command (Relay Self-Test Status) |
| 7.39 | 33 | STATUS Command Report and Definitions |
| 7.40 | 328 | SUMMARY Command |
| 7.41 | 328 | TARGET Command (Display Relay Word Bit Status) |
| 7.42 | 328 | Front-Panel LEDs and the TAR 0 Command |
| 7.43 | 329 | TFE Command (Through-Fault Event Report) |
| 7.44 | 329 | TIME Command (View/Change Time) |
| 7.45 | 330 | TRIGGER Command (Trigger Event Report) |
| 7.46 | 330 | VEC Command |
| 8.1 | 333 | Front-Panel Automatic Messages (FP_AUTO := OVERRIDE) |
| 8.2 | 335 | Front-Panel Pushbutton Functions |
| 8.3 | 343 | Possible Warning Conditions (Flashing TRIP LED) |
| 8.4 | 345 | SEL-787 Front-Panel Operator Control Functions |
| 9.1 | 350 | Event Types |
| 9.2 | 354 | Analog Event Report Columns Definitions |
| 9.3 | 361 | Digital Event Report Column Definitions |
| 9.4 | 365 | Differential Event Report Column Definitions for Analog Quantities |
| 9.5 | 366 | Differential Event Report Digital Column Definitions for Protection, Control, and |
| 10.1 | 372 | Resultant Scale Factors for Inputs |
| 10.2 | 376 | Serial Port Commands That Clear Relay Data Buffers |
| 10.3 | 377 | CTR1 Phase Current Measuring Accuracy |
| 10.4 | 378 | CTR2 Phase Current Measuring Accuracy |
| 10.5 | 378 | Power Quantity Accuracy—Wye V oltages |
| 10.6 | 379 | Power Quantity Accuracy—Delta V oltages |
| 10.7 | 380 | Periodic Relay Checks |
| 10.8 | 33 | Relay Self-Tests |
| 10.9 | 384 | Troubleshooting |
