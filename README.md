# Ignytion Whitepapers

Published whitepapers, technical lab notes, and user guides for the **Crucible** workbench and the **Igny** CLI.

This repository holds the **distributable PDFs only** — the finished artefacts that go to customers, evaluators, and the public site. Authoring sources (PPTX/HTML), the brand colour system, and the layout specification live in the internal `Docs-and-Branding` tree and are deliberately not mirrored here.

> Everything committed here is externally reachable. Only publication-approved material belongs in this repository — no source code, no internal engineering state, no unreleased claims.

---

## Contents

### Platform thesis

| Document | Pages | What it covers |
|---|---|---|
| [`Ignytion-Whitepaper.pdf`](Ignytion-Whitepaper.pdf) | 13 | *Ignytion's Guide to Open Source* — the platform thesis on infrastructure, orchestration, and the execution layer for next-generation silicon development. Covers the compute layer, orchestration and execution, the workflow surface, and observability and trust. |

### Technical lab notes — Crucible Platform Series

Each lab note runs a real toolchain end-to-end through `igny`, in a reproducible workspace, and reports measured results.

| Document | Pages | Date | What it demonstrates | Tools via `igny` |
|---|---|---|---|---|
| [`HLS_Bambu_Experiment_Ignytion_A4.pdf`](HLS_Bambu_Experiment_Ignytion_A4.pdf) | 9 | Sep 2026 | High-level synthesis: a six-line C function synthesised to Verilog RTL, then proven equivalent to the C. Two stages, both passing, reproducible from a committed `crucible.lock`. | `bambu` 2024.10, `iverilog` 14.0.0, `vvp` |
| [`Analog_CMOS_Experiments_Ignytion_A4.pdf`](Analog_CMOS_Experiments_Ignytion_A4.pdf) | 16 | Jul 2026 | Six analog CMOS experiments — MOS device modelling, logic families, amplification, oscillation. Each pairs a Python design-equation check with a SPICE deck. | `ngspice`, Python, Matplotlib |
| [`opensoc_L1_whitepaper.pdf`](opensoc_L1_whitepaper.pdf) | 13 | Jul 2026 | *OpenSoC* — a RISC-V SoC from Silicon Setu brought up on Crucible. Four experiments: RV32IM firmware build, static RTL lint, GPIO simulation, UART simulation. | `riscv-none-elf-gcc`, `verilator`, `gtkwave` |
| [`VP_Experiments_Whitepaper (1).pdf`](VP_Experiments_Whitepaper%20(1).pdf) | 14 | Jun 2026 | Virtual prototyping for RISC-V silicon: boot Linux on a virtual SoC, integrate a custom AI accelerator IP, run a quantised MNIST classifier — 100% accuracy across 740 accelerator invocations, no physical hardware. | LiteX, VexRiscv, Renode |
| [`Verilog_Experiments_Whitepaper.pdf`](Verilog_Experiments_Whitepaper.pdf) | 20 | Jun 2026 | Five foundational Verilog experiments — D flip-flop, up/down counter, seven-segment decoder, Mealy sequence detector, three-number adder. Standard lab structure throughout: Aim, Apparatus, Theory, RTL, Procedure, Observations, Result. | `iverilog`, `vvp`, `gtkwave` |
| [`Counter_experiment_whitepaper.pdf`](Counter_experiment_whitepaper.pdf) | 9 | Jun 2026 | ASIC design and verification: simulate a 4-bit counter, view its waveform, synthesise it to a standard-cell gate-level netlist. | `iverilog`, `vvp`, `gtkwave`, `yosys` + Nangate |
| [`synthesis_whitepaper.pdf`](synthesis_whitepaper.pdf) | 9 | Jun 2026 | The earlier "Day 2" revision of the counter lab note above. See [Housekeeping](#housekeeping). | as above |

### User guides

| Document | Pages | What it covers |
|---|---|---|
| [`Ignytion_Crucible_Windows_User_Guide_Updated.pdf`](Ignytion_Crucible_Windows_User_Guide_Updated.pdf) | 19 | Setting up a chip-design toolchain on Windows, running digital and analog experiments, and reproducing the same environment on any machine — without installing a tool by hand. |
| [`Ignytion_Crucible_Linux_User_Guide.pdf`](Ignytion_Crucible_Linux_User_Guide.pdf) | 13 | The Linux equivalent. See [Housekeeping](#housekeeping) — the cover needs a fix. |

---

## Authoring standards

New documents follow the specifications in the internal `Docs-and-Branding` tree:

| Standard | Governs |
|---|---|
| `Whitepapers/FORMAT.md` | Page geometry, grid, typography scale, components, page archetypes, export settings, pre-flight checklist |
| `Guides-and-Style-Docs/Ignytion-color-system-v1.md` | All colour values — authoritative |
| `Guides-and-Style-Docs/IGNYTION_Whitepaper_Style_Guide.md` | Language (Indian English), grammar, punctuation, capitalisation |

In short: **A4 portrait, 8.27 × 11.69 in**, Segoe UI and Consolas, ignition orange (`#EF9529`) used only as a functional signal, black (`#0B0B0C`) code blocks with orange commands, and a full pre-flight pass before export. Export with **File → Export → Create PDF/XPS** so the text layer survives — never Print to PDF.

---

## Adding a whitepaper

1. Author it against `FORMAT.md` in the internal tree and keep the source there.
2. Run the §14 pre-flight checklist — geometry, typography, colour, content, export.
3. Export to PDF and verify: text is selectable, code blocks are searchable, page count matches the `NN / TT` footers, nothing is clipped.
4. Copy **only the PDF** into this repository.
5. Add a row to the table above — document, page count, date, what it demonstrates, tools used.
6. Commit with a message naming the document.

### Naming

```
Topic_Descriptor_Ignytion_A4.pdf
```

Underscores, no spaces, no parenthesised suffixes such as `(1)`, no `copy` in the name. Version and date belong in the document footer and on the cover — not in the filename.

### Branches

`main` is the published set. Work lands on a topic branch (currently `whitepapers`, one commit ahead of `main` with the HLS lab note) and merges to `main` once the document is approved for release.

---

## Housekeeping

Known issues in the current set, in priority order:

1. **`Ignytion_Crucible_Linux_User_Guide.pdf` cover is broken.** Page 1 carries two overlapping 31 pt titles — `Crucible Windows User Guide` and `Crucible Linux User Guide` stacked on top of each other. The Windows title was not deleted when the Linux variant was derived. Re-export before this guide goes to anyone.
2. **The two user guides were built by different pipelines.** Windows is 19 pages from WeasyPrint (HTML); Linux is 13 pages from PowerPoint. They do not match in length or layout. Pick one pipeline and rebuild the other against it.
3. **`VP_Experiments_Whitepaper (1) copy.pdf` is a byte-identical duplicate** of `VP_Experiments_Whitepaper (1).pdf`. Delete the copy and rename the survivor per the convention above.
4. **`synthesis_whitepaper.pdf` and `Counter_experiment_whitepaper.pdf` are the same lab note.** They differ only in the "Day 2" framing and minor rewording; `Counter_experiment_whitepaper.pdf` is the later revision. Confirm which is canonical and retire the other.
5. **Filenames are inconsistent.** Only the Analog CMOS and HLS notes follow the convention. The rest should be renamed on their next revision.
6. **`Ignytion-Whitepaper.pdf` is US Letter** (8.50 × 11.00 in) while everything else is A4. Migrate it at its next major revision.

---

## Contact

- Product information — [ignytion.io](https://www.ignytion.io)
- Support and questions — [info@ignytion.io](mailto:info@ignytion.io)

© 2026 Ignytion IO
