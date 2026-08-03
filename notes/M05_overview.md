# M05: TLP Elements

**Source**: MindShare PCIe 3.1 — Module 5 (TLP Elements, ~129 MB) ⭐ MOST IMPORTANT  
**Plan**: `PCIe_project/01_PCIe_and_OS_Study_Plan.md` Module 5  
**eBook**: MindShare PCIe 3.0 — Ch 5 TLP Elements  
**Date started**: 2026-07-27  
**Date completed**:  
**Time spent**: __ h  
**Status**: Watch ✅ (2026-07-24) · eBook Ch5 ✅ (2026-07-27) · Notes in progress · Cross-ref pending

#pcie/module05

---

## One-liner

_(Fill after Pass 1 — e.g. what a TLP is and why Fmt+Type matter for DMA / PCILeech.)_

---

## Pass 1 — Outline (watch 1×)

| Topic | Notes (≤1 line) |
| ----- | --------------- |
| Why packet-based protocol | |
| TLP assembly / disassembly | |
| Generic TLP structure (Hdr + Data + Digest) | |
| Fmt + Type field | |
| 3DW vs 4DW header | |
| Memory Read / Memory Write | |
| I/O Read / Write | |
| Config Read / Write | |
| Completion (Cpl / CplD) | |
| Message TLPs | |
| First/Last DW BE (byte enables) | |
| Tag / Requester ID / Completer ID | |
| Traffic Class / Attr / Length | |
| ECRC / Digest (brief) | |
| Posted vs Non-Posted | |

### Checklist (from study plan)

- [ ] Notes: TLP Format (Fmt + Type fields)
- [ ] Notes: TLP Header layout (3DW vs 4DW)
- [ ] Notes: Memory Read, Memory Write TLPs
- [ ] Notes: I/O Read/Write, Config Read/Write
- [ ] Notes: Completion (Cpl, CplD) TLPs
- [ ] Notes: Message TLPs
- [ ] Notes: First/Last DW BE (byte enables)
- [ ] Notes: Tag, Requester ID, Completer ID
- [ ] Cross-ref: `pcileech_pcie_tlp_a7.sv`
- [ ] Cross-ref: `pcileech_tlps128_bar_controller.sv`
- [ ] eBook Ch 5: deep read
- [ ] Blog: "PCIe TLP Format Decoded: Reading Hex Like a Pro"

---

## Key concepts

### Generic TLP structure

-

-

### Fmt + Type

| Fmt | Meaning | Notes |
| --- | ------- | ----- |
|  |  |  |
|  |  |  |

| Type (examples) | Request / Cpl | Notes |
| --------------- | ------------- | ----- |
|  |  |  |
|  |  |  |

### 3DW vs 4DW header

-

-

### Memory / I/O / Config / Completion / Message

| TLP | Posted? | Data? | Notes |
| --- | ------- | ----- | ----- |
| MRd |  |  |  |
| MWr |  |  |  |
| I/O Rd/Wr |  |  |  |
| Cfg Rd/Wr |  |  |  |
| Cpl |  |  |  |
| CplD |  |  |  |
| Msg / MsgD |  |  |  |

### Byte enables (First/Last DW BE)

-

-

### IDs and Tag

| Field | Role | Notes |
| ----- | ---- | ----- |
| Requester ID |  |  |
| Completer ID |  |  |
| Tag |  |  |

---

## Cross-ref: PCILeech TLP path

**Files** (summarize after reading):

- `pcileech_pcie_tlp_a7.sv` —
- `pcileech_tlps128_bar_controller.sv` —

| Topic in M05 | File / signal | What to look for |
| ------------ | ------------- | ---------------- |
| TLP RX/TX |  |  |
| Header parse |  |  |
| MRd / MWr handling |  |  |
| BAR hit |  |  |
| Completions |  |  |

**Attack / detection angle** _(for research)_:

-

---

## Spec & terminology

| Term | Meaning | Notes |
| ---- | ------- | ----- |
| TLP |  |  |
| Fmt |  |  |
| Type |  |  |
| DW |  |  |
| BE |  |  |
| Posted |  |  |
| Non-Posted |  |  |
| Cpl / CplD |  |  |
| Tag |  |  |
| RID / CID |  |  |
| TC |  |  |
| ECRC |  |  |

---

## Diagrams / SVG

_(Optional — generic header layout, MRd→CplD sequence.)_

- [x] Header field map — ![[diagrams/tlp_header.svg]]
- [ ] Memory Read + Completion with Data sequence

*Source file: `diagrams/tlp_header.svg` — edit in Obsidian (open SVG) or any text/vector editor.*

---

## Related modules

- Prerequisite: [[M02a_overview]] / [[M02b_overview]]
- Prerequisite: [[M03_config_space]] *(create if needed)*
- Prerequisite: [[M04_routing]] *(create if needed)*
- Next: [[M06_flow_control]] *(create when Module 6 starts)*
- eBook: Ch 5 TLP Elements

---

## Questions / unclear

1. 
2. 

*Re-watch timestamps:*

---

## Blog outline

**Working title**: PCIe TLP Format Decoded: Reading Hex Like a Pro

1. Hook:
2. 
3. 
4. PCILeech / DMA example:
5. Takeaway:

---

## Completion log

| Step | Done | Date |
|------|------|------|
| Watch 1× | [x] | 2026-07-24 |
| Pass 1 outline | [ ] | |
| Notes Pass 2 | [ ] | |
| Re-watch hard parts | [ ] | |
| PCILeech cross-ref read | [ ] | |
| PCILeech cross-ref in notes | [ ] | |
| eBook Ch 5 | [ ] | |
| Blog draft | [ ] | |
| TLP Encoder/Decoder RTL | [ ] | |
