# M02b: PCIe Overview Part 2

**Source**: MindShare PCIe 3.1 — Module 2b (Overview Part 2, ~73 MB)  
**Plan**: `PCIe_project/01_PCIe_and_OS_Study_Plan.md` Module 2b  
**Date started**:  
**Date completed**:  
**Time spent**: __ h  
**Status**: Watch ✅ · Cross-ref read ✅ · Notes in progress

#pcie/module02b

---

## One-liner

_(Fill after Pass 1.)_

---

## Pass 1 — Outline (watch 1×)

| Topic | Notes (≤1 line) |
| ----- | --------------- |
| Bandwidth calculation | |
| Encoding overhead (8b/10b vs 128b/130b) | |
| Gen1 → Gen6 rates | |
| Link width × speed → payload BW | |
| Bidirectional / duplex | |
| Differential signaling (brief) | |
| Embedded clock / CDR (brief) | |
| Peak table vs real payload BW | |
| PCILeech: Gen2 x1 implication | |

### Checklist (from study plan)

- [ ] Notes: Bandwidth calculation
- [ ] Notes: Generations (Gen1 to Gen6)
- [x] Cross-ref read: PCILeech runs PCIe Gen2 x1 (`vmd_top.sv`)
- [ ] Cross-ref summarized in section below

---

## Key concepts

### Bandwidth calculation

_(How to go from GT/s + encoding + link width → approximate usable MB/s / GB/s.)_

-

-

### Generations (Gen1 to Gen6)

| Gen | GT/s | Encoding | Notes |
| --- | ---- | -------- | ----- |
| Gen1 | 2.5 | 8b/10b | |
| Gen2 | 5.0 | 8b/10b | |
| Gen3 | 8.0 | 128b/130b | |
| Gen4 | 16 |  | |
| Gen5 | 32 |  | |
| Gen6 | 64 |  | |

### Aggregate bandwidth table _(optional — fill from video/eBook)_

| Width | Gen1 | Gen2 | Gen3 |
| ----- | ---- | ---- | ---- |
| ×1 |  |  |  |
| ×4 |  |  |  |
| ×8 |  |  |  |
| ×16 |  |  |  |

---

## Cross-ref: PCILeech = Gen2 x1 Endpoint

**Read ✅** `vmd_top.sv` + README (San Jose 2026-07-13) — summarize here:

- Link speed / width configured as:
- Evidence in RTL / README:
- Why Gen2 x1 is enough for DMA attack research:
- Any hard-coded Gen/width in the RTL:

---

## Spec & terminology

| Term | Meaning | Notes |
| ---- | ------- | ----- |
| GT/s |  |  |
| Aggregate GB/s |  |  |
| 8b/10b |  |  |
| 128b/130b |  |  |
| Full duplex |  |  |

---

## Diagrams / SVG

_(Optional — bandwidth bar chart or Gen timeline.)_

---

## Related modules

- Prerequisite: [[M02a_overview]]
- Prerequisite: [[M01_background]]
- Next: [[M03_config_space]] *(create when Module 3 starts)*

---

## Questions / unclear

1. 
2. 

*Re-watch timestamps:*

---

## Blog outline

Continue Blog: "PCIe Topology Explained with Real Examples" (or separate bandwidth post)

1. Hook:
2. 
3. 
4. PCILeech Gen2×1 example:
5. Takeaway:

---

## Completion log

| Step | Done | Date |
|------|------|------|
| Watch 1× | [x] | 2026-07-13 |
| Pass 1 outline | [ ] | |
| Notes Pass 2 | [ ] | |
| Re-watch hard parts | [ ] | |
| PCILeech cross-ref read | [x] | 2026-07-13 |
| PCILeech cross-ref in notes | [ ] | |
| Blog draft | [ ] | |
