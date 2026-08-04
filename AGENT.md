# AGENT.md — SEL-787 Workspace Guide

> Onboarding notes for the AI assistant. Read this first when starting a new
> session in this workspace so you know the context, available resources, and how
> to answer questions efficiently.

## Workspace purpose
This folder supports work with the **SEL-787 Transformer Protection Relay**
(Schweitzer Engineering Laboratories). The user is reading the product manual and
will ask questions about the relay — protection functions, settings, SELOGIC,
communications, testing, front-panel/display config, etc.

## Key files in this workspace

| File | What it is | Use it for |
|---|---|---|
| `787_IM_20260130.pdf` | Full product instruction manual (640 pages, PM787-01, date code 20260130) | Source of truth; extract text or render figures from it |
| `SEL-787_Reference.md` | Distilled reference of the manual | **First stop** for specs, settings, differential logic, CTC, commands, access levels |
| `SELOGIC_101_Lesson.md` | SELOGIC tutorial + worked examples + exercises w/ answer key | Anything about SELOGIC control equations |
| `figtable_index.md` | Index of all ~200 figures & ~169 tables → PDF page numbers | Locate/render a specific figure or table |
| `AGENT.md` | This file | Onboarding |

## Answer lookup order (do this when the user asks a question)
1. **`SEL-787_Reference.md`** — fast, pre-distilled answers.
2. **`SELOGIC_101_Lesson.md`** — if the question is SELOGIC/logic related.
3. **`figtable_index.md`** — if the answer is in a diagram/table; get its PDF page.
4. **The full PDF** — for deeper detail not in the summaries (see extraction below).
5. **Web search (Brave)** — ONLY if the answer is not in the manual at all.

## How to read the PDF (text)
The PDF is binary; extract text with Python + pypdf (already installed):
```powershell
python -c "import pypdf; r=pypdf.PdfReader('787_IM_20260130.pdf'); open('tmp.txt','w',encoding='utf-8').write(''.join(f'\n===== PAGE {i+1} =====\n'+(p.extract_text() or '') for i,p in enumerate(r.pages)))"
```
Then read/search `tmp.txt`. **Delete `tmp.txt` when done** (keep the workspace clean).

## How to read a DIAGRAM (render to image)
Text extraction only captures figure *labels*, not the visual. To actually see a
diagram, render its page to an image (PyMuPDF/`fitz` is installed):
```powershell
python -c "import fitz; d=fitz.open('787_IM_20260130.pdf'); d[PDFPAGE-1].get_pixmap(dpi=200).save('fig.png')"
```
- Look up the figure's PDF page in `figtable_index.md` first.
- Pages are 0-indexed in code, so use `PDFPAGE-1`.
- A figure may render one page off if its caption sits at a page boundary — check
  the adjacent page if needed.
- Then view `fig.png` to analyze/answer. Clean up the PNG afterward.

## Web search (Brave) — how it actually works here
- The assistant's built-in `brave_web_search` tool has a DEAD key in-session
  (returns `SUBSCRIPTION_TOKEN_INVALID`) — **do not rely on it**.
- The user has a WORKING Brave API key configured in `.mcp.json`. Run searches via
  the terminal with that key:
```powershell
$r = Invoke-WebRequest -Uri "https://api.search.brave.com/res/v1/web/search?q=YOUR+QUERY" -Headers @{ "X-Subscription-Token" = "<USER_KEY_FROM_.mcp.json>"; "Accept" = "application/json" } -UseBasicParsing
($r.Content | ConvertFrom-Json).web.results | Select-Object -First 6 title,url,description | ConvertTo-Json | Out-File brave_results.json -Encoding utf8
```
  Then read `brave_results.json` and clean it up after.
- The key lives in `C:\Users\<user>\.mcp.json` and the solution-level `.mcp.json`.
  ⚠️ These files contain a plaintext API key and sit in OneDrive + a Git repo —
  suggest adding `.mcp.json` to `.gitignore`; do NOT paste the key into chat.
- For authoritative SEL info, `selinc.com` pages are often login-gated/JS-rendered
  and may not extract cleanly; prefer the local PDF when possible.

## MCP / Visual Studio setup notes (already configured)
- Environment: **Visual Studio Enterprise 2026 (18.4.1)**, GitHub Copilot Agent mode.
- MCP config file name: **`.mcp.json`** (locations: solution folder, and global
  `C:\Users\<user>\.mcp.json`).
- The `${input:...}` prompt method did NOT prompt in the user's VS build, so the
  Brave key is currently **hardcoded** in `env.BRAVE_API_KEY` (this works).
- Node.js v22 + npx are installed; the Brave MCP package
  `@modelcontextprotocol/server-brave-search` launches fine on stdio.

## Environment quick facts
- OS shell: PowerShell (Windows). Use `;` to separate commands, one command at a time.
- Python 3.13 available; `pypdf` and `pymupdf` (fitz) installed.
- Node v22 / npx available.
- Workspace path: `C:\Users\<user>\OneDrive - Halliburton\Documents\Hydra\SEL-787`
- Repo root (Git): one level up (`...\Hydra`).

## Working style the user prefers
- Be direct and honest about tool limitations (don't over-explain or make excuses).
- Keep the workspace clean: delete temp files (`tmp.txt`, `*.png`, `brave_results.json`) after use.
- Verify claims with the actual PDF/tools rather than guessing.
- Cite the figure/table/section when answering from the manual.

## Quick reference — common SEL-787 landmarks (PDF pages)
- Fig 4.1 Percentage Restraint Differential Characteristic → p.94
- Fig 4.4 Differential Element Decision Logic → p.96
- Fig 4.5 Differential Harmonic Blocking Logic → p.98
- Table 4.4 Differential Element Settings → p.98
- Table 4.5 WnCTC Compensation Matrices → p.102
- Fig 2.21 Two-Winding Transformer Protection w/ REF → p.68
- Fig 10.1 Low-Level Test Interface (J2/J3) → p.372
- Appendix A: Firmware/ICD/Manual versions (firmware revision history)
- Appendix J: Relay Word Bits (all valid SELOGIC operand names)
- Appendix L: Protection Application Examples (CTC/TAP worked examples)
- Command Summary + default passwords (L1 OTTER / L2 TAIL / C CLARKE): Section 7
