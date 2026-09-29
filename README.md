# Ignytion Whitepapers

Published whitepapers, technical lab notes, and user guides for the **Crucible** workbench and the **Igny** CLI.

This repository is a **superproject**. Each module's documents live in their own repository, checked out here as a submodule, so a document set can be versioned, branched, and released independently of the others. Document sets with no paired experiment repository stay as plain folders.

---

## Layout

### Module document repositories (submodules)

Each of these is a standalone repository paired with the experiment repository that produced its documents.

| Folder | Document repository | Paired experiment repository |
|---|---|---|
| `analog_whitepapers/` | `ignytion_ae/analog_whitepapers` | `ignytion_ae/analog_experiments` |
| `verilog_whitepapers/` | `ignytion_ae/verilog_whitepapers` | `ignytion_ae/verilog_experiments` |
| `synthesis_whitepapers/` | `ignytion_ae/synthesis_whitepapers` | `ignytion_ae/apb_rtl_to_gds` |
| `opensoc_whitepapers/` | `ignytion_ae/opensoc_whitepapers` | `ignytion_ae/silicon-sethu` |
| `python4vlsi_whitepapers/` | `ignytion_ae/python4vlsi_whitepapers` | `ignytion_ae/python4vlsi` |
| `hls_whitepapers/` | `ignytion_ae/hls_whitepapers` | `ignytion_ae/HLS_experiment` |
| `vp_whitepapers/` | `ignytion_ae/vp_whitepapers` | `core_product/crucyble_flow_demo/crucible-vp-demos` ⚠ |

⚠ `vp_whitepapers` is the one pair that crosses group boundaries — its documents sit in `ignytion_ae`, its demos in `core_product/crucyble_flow_demo`. Access to one does not imply access to the other, and that group's external-reach clearance should be confirmed separately.

### Plain folders

These sets have no paired experiment repository and are tracked directly here. Split them out the same way if and when one appears.

| Folder | Contents |
|---|---|
| `platform_thesis/` | `Ignytion-Whitepaper.pdf` — the platform thesis |
| `user_guides/` | Crucible Windows and Linux user guides |

---

## Cloning

```bash
git clone --recurse-submodules https://gitlab.com/ignytion_io-group/ignytion_ae/whitepapers.git
```

In an existing clone:

```bash
git submodule update --init --recursive
```

`--recursive` is safe here. Each document repository carries its paired experiment repository as a submodule declared with `update = none`, so a recursive checkout stops at that boundary instead of following the pair back and forth forever. To pull an experiment tree in deliberately:

```bash
cd analog_whitepapers
git submodule update --init -- analog_experiments
```

### Why the pairing is circular

A document is only reproducible if you can get back to the exact experiment revision behind its numbers, and an experiment is only publishable if you can find the write-up that reports it. Both directions are load-bearing, so each side pins the other:

```text
whitepapers/                            (superproject)
└── analog_whitepapers/                 submodule, normal update
    ├── Analog_CMOS_Experiments_…pdf
    └── analog_experiments/             submodule, update = none  ← recursion stops here
        └── analog_whitepapers/         declared, never fetched by the line above
```

The `update = none` guard lives on the document-repository side only. One guard is enough to break the cycle from any entry point.

---

## Scope and publication boundary

> **This repository is externally reachable.** Everything committed here, and everything reachable through its submodules, is publishable material.

The document repositories hold **exported PDFs only** — never PPTX or HTML authoring sources, which stay in the internal `Docs-and-Branding` tree along with the colour system and the layout specification.

The paired experiment repositories **do** contain source code: RTL, SPICE decks, testbenches, and build scripts for the experiments each document reports. That is deliberate — these are the open experiment suites the documents are written about. It is also a change from this repository's earlier contract, which was PDFs only and excluded source code outright.

Two things have **not** loosened:

- `crucible-core` is never referenced from here, directly or through a submodule.
- Internal engineering state — project context trees, unreleased claims, secrets, credentials — belongs nowhere in this graph.

Before adding a submodule, confirm the target repository is cleared for external reach on its own merits. Publishing is irreversible: caches and indexes survive deletion.

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
4. Copy **only the PDF** into the module's document repository and commit it there.
5. Add a row to that repository's own README table — document, page count, date, what it demonstrates, tools used.
6. From this superproject, stage the moved submodule pointer and commit it:

   ```bash
   git add <module>_whitepapers
   git commit -m "Advance <module>_whitepapers to <document>"
   ```

### Naming

```
Topic_Descriptor_Ignytion_A4.pdf
```

Underscores, no spaces, no parenthesised suffixes such as `(1)`, no `copy` in the name. Version and date belong in the document footer and on the cover — not in the filename.

### Adding a new module

1. Create `<module>_whitepapers` in `ignytion_ae` and commit the PDFs there.
2. Add the paired experiment repository inside it as a submodule with `update = none`.
3. Add the document repository to the experiment repository as a normal submodule.
4. Register it here: `git submodule add -b main <url> <module>_whitepapers`.

### Branches

`main` is the published set. Work lands on a topic branch (currently `whitepapers`) and merges to `main` once the document is approved for release. Submodule pointers move under the same discipline — never advance a pointer on `main` to an unreleased document revision.

---

## Housekeeping

Known issues in the current set, in priority order. Items inside a submodule are fixed in that repository and land here as a pointer bump; each document repository's own README repeats the items that belong to it.

**In this repository**

1. **`user_guides/Ignytion_Crucible_Linux_User_Guide.pdf` cover is broken.** Page 1 carries two overlapping 31 pt titles — `Crucible Windows User Guide` and `Crucible Linux User Guide` stacked on top of each other. The Windows title was not deleted when the Linux variant was derived. Re-export before this guide goes to anyone.
2. **The two user guides were built by different pipelines.** Windows is 19 pages from WeasyPrint (HTML); Linux is 13 pages from PowerPoint. They do not match in length or layout. Pick one pipeline and rebuild the other against it.
3. **`platform_thesis/Ignytion-Whitepaper.pdf` is US Letter** (8.50 × 11.00 in) while everything else is A4. Migrate it at its next major revision.

**In the document submodules**

4. **`vp_whitepapers`: `VP_Experiments_Whitepaper (1) copy.pdf` is a byte-identical duplicate** of `VP_Experiments_Whitepaper (1).pdf`. Delete the copy and rename the survivor per the convention above.
5. **`synthesis_whitepapers` holds the counter lab note twice.** `synthesis_whitepaper.pdf` and `Counter_experiment_whitepaper.pdf` differ only in the "Day 2" framing and minor rewording; the latter is the later revision. Confirm which is canonical and retire the other. The newer `APB_RTL_to_GDS_Ignytion_A4.pdf` supersedes neither — it covers the full flow, not the counter.
6. **`python4vlsi_whitepapers` has no document yet.** The lab series exists in the paired repository; the write-up is still to be authored.
7. **Filenames are inconsistent.** Only the Analog CMOS, HLS, and APB RTL-to-GDS notes follow the convention. The rest should be renamed on their next revision.

**Repository naming**

8. **`HLS_experiment` breaks the naming pattern** used by its siblings (`analog_experiments`, `verilog_experiments`) — capitalised and singular. Renaming it to `hls_experiments` would mean updating the submodule name, path, and URL in `hls_whitepapers`, and its own `origin`. Left as-is until decided.

---

## Contact

- Product information — [ignytion.io](https://www.ignytion.io)
- Support and questions — [info@ignytion.io](mailto:info@ignytion.io)

© 2026 Ignytion IO
