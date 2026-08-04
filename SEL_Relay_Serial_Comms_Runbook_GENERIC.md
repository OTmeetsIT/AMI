# SEL Relay Serial Communications Runbook (via serial MCP) — GENERIC

> **Read this file first before talking to an SEL relay over serial.**
> It captures a working procedure derived from hands-on testing so a new session can drive a
> relay reliably without repeating the trial-and-error.
>
> **Scope:** validated on an SEL-787-class relay. The transport behavior (CR terminator, hex
> writes, access levels, chunked reads, failure modes) applies broadly to the SEL ASCII command
> family. **Model-specific commands, settings names, and defaults must be confirmed against the
> instruction manual for the unit you are actually connected to.**
>
> **No unit-specific data belongs in this file.** Record serial numbers, FIDs, IP addresses,
> settings values, and site details in a separate per-unit profile (template in §9).

---

## 0. TL;DR — the golden rules

1. **Port: DO NOT assume a fixed port name.** A USB-serial adapter can enumerate to a different
   port (COM number on Windows, `/dev/tty.*` or `/dev/ttyUSB*` on macOS/Linux) depending on which
   physical USB port is used. **Always run `serial_list_ports` first, then ASK THE USER to confirm
   which listed port is the relay** before opening. Identify the adapter by its VID:PID, but if
   more than one matching adapter is present, the user must say which one is the relay.
2. **Config:** confirm against the relay's port settings — **9600-8-N-1, no flow control** is the
   common factory default, but the port may have been reconfigured (19200/38400 are also typical).
3. **Line terminator MUST be a bare carriage return `0D` (CR).** NOT `\n` (0x0A), NOT a literal `\r`.
4. **Send every command as HEX** with a trailing `0D`. Text mode sends the wrong terminator and fails.
5. **The relay logs out on any USB re-enumeration / session drop** — re-run the access-level
   sequence to get back to Level 1.
6. **Only one program can own the port** (exclusive access). Close any terminal emulator or vendor
   settings software before opening via MCP, and vice-versa.
7. **Always `serial_close` when done** to avoid leaving a zombie handle that locks the port
   ("Access is denied").

---

## 1. Port / connection settings

> ⚠️ **The port name is NOT fixed.** Always `serial_list_ports` and have the **user confirm which
> port is the relay** before opening. Any port name in examples below is a **placeholder** —
> substitute the port the user verifies for this session.

| Setting | Value |
|---|---|
| Port | **Verify each session** via `serial_list_ports` + user confirmation |
| Adapter | USB-serial adapter — note its VID:PID to distinguish it from other devices |
| Baud rate | Per relay port settings (commonly 9600) |
| Data bits | 8 |
| Parity | none |
| Stop bits | 1 |
| Flow control | none |

`serial_open` example params:
`port=<verified port>, baud_rate=<verified>, data_bits=8, parity=none, stop_bits=1, flow_control=none`

If the relay does not respond to a bare `0D` probe, a **baud-rate mismatch** is the first thing to
suspect — step through 9600 / 19200 / 38400 before assuming a hardware fault.

---

## 2. The critical detail: line terminator

SEL relays terminate ASCII commands with a **carriage return (CR = 0x0D)**.

Proven empirically with a two-port capture test: typing a command + Enter in a terminal put the
ASCII bytes followed by a single `0D` on the wire — i.e. **Enter = one `0D` byte, no line feed**.

**Failure modes observed:**
- Sending text `"<CMD>\r"` → tool emitted a literal backslash + `r` (bytes `5C 72`) → relay ignores it.
- Sending text `"<CMD>\n"` → tool emitted LF (`0A`) → relay echoes partially, then stalls.

**Fix:** send **hex** with a trailing `0D`.

---

## 3. Command hex cheat-sheet (append `0D` = CR)

> ℹ️ **This table is only the common SEL ASCII subset.** For the full command set, access-level
> map, and any model-specific commands, consult **the Command Summary section of the instruction
> manual for your specific relay model**, plus the Relay Word Bit appendix for valid SELOGIC
> operand names.
>
> To drive any command not listed below, convert its ASCII to hex and append `0D` (see the build
> tip under the table).

| Command | Meaning | Hex to send (encoding = hex) |
|---|---|---|
| `<Enter>` | bare CR (probe prompt) | `0D` |
| `ACC` | enter Access Level 1 | `4143430D` |
| `2AC` | enter Access Level 2 | `3241430D` |
| `STA` | self-test / status | `5354410D` |
| `MET` | metering | `4D45540D` |
| `TAR R` | reset target LEDs | `54415220520D` |
| `HIS` | event history | `4849530D` |
| `SHO 1` | show Group 1 settings | `53484F20310D` |
| `SHO P 1` | show Port 1 settings | `53484F205020310D` |
| `SHO F` | show Front-Panel settings | `53484F20460D` |
| `SHO L` | show SELOGIC settings | `53484F204C0D` |
| `QUI` | log out | `5155490D` |

### Factory-default passwords

These are the published SEL factory defaults. Send them exactly like any other command — ASCII
bytes + `0D`.

| Level | Default password | Hex to send |
|---|---|---|
| Level 1 (`ACC`) | `OTTER` | `4F545445520D` |
| Level 2 (`2AC`) | `TAIL` | `5441494C0D` |
| Calibration (`CAL`) | `CLARKE` | `434C41524B450D` |

> ⚠️ **If a default password is rejected, the password has been changed on that unit.** Do not
> retry or attempt to guess it — repeated failures can trip the relay's unauthorized-access alarm
> and lock out further attempts. Stop and ask the user for the correct credentials for that relay,
> and record them in the unit profile (§9).

> **Calibration level is not a routine access level.** `CAL` exposes factory calibration and can
> invalidate the relay's metering accuracy. Do not enter it unless the user has specifically asked
> for it and knows why.

> To build hex for any other command: take the ASCII bytes of the command text and append `0D`.
> e.g. `SHO 2` = `53 48 4F 20 32` + `0D` → `53484F20320D`.
>
> **Formatting tip:** the `hex` encoder accepts **space-separated byte pairs** as well as a solid
> string — `41 43 43 0D` is equivalent to `4143430D`. Spaced pairs are easier to read and audit
> when hand-building a command. (The server's read side also *emits* hex as spaced pairs.)

---

## 4. Access levels & prompts

| Prompt | Level | How to reach | Capabilities |
|---|---|---|---|
| `=` | 0 | default on connect | very limited |
| `=>` | 1 | `ACC` + Level 1 password (default `OTTER`) | read-only reports: `STA`, `MET`, `TAR`, `HIS`, `EVE`, `SHO`; `TAR R` |
| `=>>` | 2 | `2AC` + Level 2 password (default `TAIL`) | state-changing: `SET`, `OPE`, `CLO`, `PUL`, password changes |

**If a factory-default password is rejected, it has been changed on that unit** — get the real one
from the user rather than guessing (see §3).

**Important:** after any USB re-enumeration or session timeout the relay drops back to **Level 0**
(`=` prompt). Re-authenticate before issuing level-gated commands, or you get `Invalid Access Level`.

> ⚠️ **Level 2 can change relay behaviour and operate outputs.** Do not enter Level 2, and never
> issue `SET`/`OPE`/`CLO`/`PUL`, without confirming with the user first — see §10.

---

## 5. Response framing

- The relay **echoes the command** back first (e.g. you'll see `ACC\r` in the reply).
- Report bodies are wrapped in control bytes: **STX `\u{2}` (0x02)** at the start and **ETX
  `\u{3}` (0x03)** at the end, followed by the prompt.
- The trailing prompt tells you the level: `=` / `=>` / `=>>`.

Example shape (login success):
```
*****\r\n\r\n<model banner> ... Level 1\r\n\u{3}\u{2}\r\n=>\u{3}
```

---

## 6. Reading long reports (chunked reads)

Settings dumps (`SHO 1`, `SHO P 1`, `SHO F`, `SHO L`) are large (several KB) and stream **slowly at
low baud rates**. A single read often times out or grabs only the first chunk.

**Procedure:**
1. `serial_write` the command (hex).
2. `serial_read` with generous timeouts, e.g.
   `initial_timeout_ms=8000, idle_timeout_ms=2500-3000, max_bytes=8192`.
   - **Raise `max_bytes` toward 8192** (the server's default `max_buffer_size` is **8192** bytes) to
     pull each report in fewer round-trips rather than small 2048-byte chunks.
3. **Repeat `serial_read`** back-to-back to walk the stream until you reach the prompt.
4. Reports can start **repeating from the top** when complete — that's the signal you have the
   whole thing.

**Buffer carryover gotcha:** if the previous command's output hadn't fully drained, your next read
may return **leftover bytes from the prior report**. Keep reading until you see the new command's
echo followed by its body.

---

## 7. Known instability: hangs, port lock-up, and the "first-write-cancel"

There are **two distinct failure modes**. Diagnose which one you have before "fixing" it.

> ⚠️ **PARTIALLY UNVERIFIED — read before touching hardware.**
>
> Evidence suggests the "first-write-cancel" / "Access is denied" lock-ups can be caused by the
> **`serial-mcp-server` process leaking the OS port handle after a failed write** — not necessarily
> by a relay-side UART hang. Observed pattern:
> - After a canceled write, the server log shows `Failed to write ... Access is denied (os error 5)`,
>   then a `serial_close` that reports success but does **not** release the handle; every subsequent
>   `serial_open` returns "Access is denied".
> - A terminal emulator **and** a raw OS-level serial open both opened the same port instantly once
>   the `serial-mcp-server` process was killed — proving adapter, cable, and relay were all fine and
>   the lock lived in the MCP server process.
> - Restarting the MCP server alone fully restored comms in at least one session.
>
> **This has NOT disproved the relay-hang theory in 7B** — in earlier sessions both variables were
> changed at once and never isolated. **TODO — controlled A/B test:** the next time a
> first-write-cancel occurs, try **ONLY** restarting the MCP server (no replug, no relay
> power-cycle). If that alone clears it, the root cause is the handle leak and 7B should be
> rewritten. Until then, treat "restart the MCP server first" as the **cheapest first step**.
>
> **Mechanism (from the server's theory-of-operation doc):**
> - Each connection is a single **`Arc<Mutex<SerialStream>>`**. If a `write` blocks on a hung port
>   **while holding that mutex**, every subsequent read/write on that connection waits on the same
>   lock and is then torn down — exactly the "first write cancels, then everything cancels" cascade.
> - The open path checks an **in-memory `HashMap` of connections** and returns **`ConnectionExists`**
>   if the entry is still present. That is a **different failure** from the OS `Access is denied`
>   (os error 5): `ConnectionExists` = the *server's own pool* never dropped the entry;
>   `Access is denied` = the *OS handle* wasn't released. Both clear on server restart.
> - **"A task was canceled" is NOT a normal timeout.** `ReadTimeout` is returned as a non-error
>   "Timeout response" — so a *cancel* is the async future being dropped (stuck write / mutex
>   teardown), i.e. a **server/transport** symptom, not relay silence.
> - Shutdown runs **`close_all()`** over the HashMap, so a **graceful stop** releases every handle;
>   a **hard kill may skip that cleanup**. Prefer the graceful reload when recovering.

### 7A. PC-side port lock-up (zombie handle)

**Symptom progression during long sessions:**
1. Reads start timing out ("Failed").
2. Then writes fail too ("Failed" / "A task was canceled").
3. Then `serial_open` fails with **"Access is denied"** (zombie handle not released).
4. `serial_list_ports` may show the port **disappear or change name** — the adapter re-enumerated.

**Recovery:**
1. `serial_close` the dead handle.
2. Restart the MCP server (graceful reload preferred).
3. Retry `serial_open` a few times — the driver may need 30–60 s to release.
4. If still "Access is denied": **unplug the USB adapter, wait ~10 s, replug** (ideally a different
   USB port or a **powered hub**). Note the new port name.
5. Re-open, then **re-authenticate** — the session reset to Level 0.

**Mitigation:** marginal USB power or USB selective suspend. On Windows: Device Manager → the
adapter's COM port → Power Management → uncheck "Allow the computer to turn off this device". A
powered hub helps on any OS.

**Best practice:** always `serial_close` explicitly when finished; don't leave the handle open to
idle-timeout. A clean close prevents the *Access-denied* lock even across long idle periods.

### 7B. ⭐ Relay-side serial hang (the "first-write-cancel")

> ⚠️ **See the flag at the top of §7.** The "relay power-cycle is the only fix" claim is now **in
> doubt** — restarting the MCP server alone restored comms in a later session. **Restart the MCP
> server before power-cycling anything.**

**Symptom:** `serial_open` **succeeds**, but the **first `serial_write` (even a single `0D`) fails
with "A task was canceled"**. Reads then cancel too, and the next open may go "Access is denied".

**Historically attributed to the RELAY's serial port hanging, not the PC.** In the original session
this was reached by elimination:
- Fresh MCP process (full restart) → ❌ still canceled
- USB adapter replug (multiple times) → ❌ still canceled
- Relay power-cycle → ✅ fixed (bare `0D` immediately returned the `=` prompt)

**Recovery order (cheapest first):**
1. `serial_close` any handle.
2. **Restart the MCP server** (graceful reload, not a hard kill).
3. Unplug the USB adapter — clears the OS "Access is denied" lock from the canceled write.
4. Replug → `serial_list_ports` → confirm port with user → open → `0D` probe → expect `=`.
5. **Only if steps 1–4 fail:** power-cycle the relay.
   > 🚨 **Never power-cycle a relay without confirming it is out of service.** On an in-service
   > relay this removes protection from the equipment it is guarding. This step requires explicit
   > authorisation from whoever owns the asset — it is not the agent's call.
6. Re-authenticate after any of the above.

**Quick way to split PC-vs-relay:** try a terminal emulator on the port and press Enter. If it also
hangs on the first keystroke, suspect the relay or cable. If it gets a prompt, it's the tool —
restart the MCP server. A raw OS-level open is a faster non-interactive check (run with the MCP
server stopped). Windows/PowerShell example:

```powershell
try { $p = New-Object System.IO.Ports.SerialPort('<PORT>',9600,'None',8,'One'); $p.Open(); 'PORT FREE'; $p.Close(); $p.Dispose() } catch { "LOCKED: $($_.Exception.Message)" }
```

macOS/Linux equivalent: `python3 -c "import serial;serial.Serial('<PORT>',9600).close();print('PORT FREE')"`

If the raw open says **PORT FREE** while MCP says "Access is denied" or `ConnectionExists` → the
lock is in the MCP server; restart it gracefully. No relay power-cycle needed.

---

## 8. Verified working sequence (copy/paste flow)

```
1. serial_list_ports                       # list ports
   -> ASK THE USER which listed port is the relay (never assume)
2. serial_open <verified port> <verified baud>-8-N-1
3. serial_write hex 0D                     # probe -> expect '=' (Level 0)
4. serial_read
5. serial_write hex 4143430D               # ACC
6. serial_read                             # -> "Password: ? "
7. serial_write hex 4F545445520D           # OTTER (factory default L1)
8. serial_read                             # -> "Level 1 ... =>"
   # If rejected: password has been changed -> ask the user, do not guess
9. serial_write hex 5354410D               # STA (or any read-only command)
10. serial_read (repeat as needed)         # walk the stream
... more commands ...
N. serial_close                            # ALWAYS close when done
```

---

## 9. Unit profile — fill in per relay (keep OUT of this generic file)

Create a separate `<unit-name>_profile.md` alongside this runbook and record:

| Field | Value |
|---|---|
| Model / part number | |
| Serial number | |
| FID string | |
| Terminal ID (TID) / Relay ID (RID) | |
| **Bench/test unit or IN SERVICE?** | |
| Serial port settings (baud etc.) | |
| Level 1 / Level 2 credentials | *(note whether factory defaults still apply; if changed, store securely — not in a plaintext repo)* |
| Network settings (if applicable) | |
| Active settings group & key protection settings | |
| CT/PT ratios, winding config | |
| Front-panel / target LED assignments | |
| Known-benign self-test alarms & why | |

**Capture the as-found state before changing anything.** From Level 1, `SHO 1` / `SHO P 1` /
`SHO F` / `SHO L` and `STA` give a complete baseline; save the raw output.

**Interpreting self-test alarms:** an `STA` alarm may be genuine or an expected artefact of a
partial bench setup (e.g. an RTD input enabled with no RTDs wired). **Do not assume it is benign** —
confirm against the unit profile or ask the user.

---

## 10. Quick reference: what NOT to do

- ❌ Don't assume a port name — `serial_list_ports` and have the user confirm the port every session.
- ❌ Don't assume the baud rate — verify it, and suspect a mismatch first if the `0D` probe is silent.
- ❌ Don't send commands in text mode with `\r` or `\n` — use hex + `0D`.
- ❌ Don't assume you're still logged in after any gap — check the prompt (`=` vs `=>` vs `=>>`) and
  re-authenticate if needed.
- ❌ Don't open the MCP connection while a terminal emulator or vendor settings software holds the
  port (and vice-versa).
- ❌ Don't leave the port open when finished — always `serial_close`.
- ❌ **Don't enter Level 2 or issue any state-changing command (`SET`, `OPE`, `CLO`, `PUL`, password
  changes) without explicit confirmation from the user.** Read-only commands are safe; these are not.
- ❌ **Don't assume the relay is a bench unit.** Confirm its service status before power-cycling,
  operating outputs, resetting targets, or changing settings. When in doubt, treat it as in service
  and stop to ask.
