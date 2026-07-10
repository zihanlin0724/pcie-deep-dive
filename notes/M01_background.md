# M01: PCIe Background Info

**Source**: MindShare PCIe 3.1 — Module 1 (Background Info, ~96 MB)  
**Plan**: `PCIe_project/01_PCIe_and_OS_Study_Plan.md` Module 1  
**Date started**: 2026-07-09  
**Time spent**: __ h  
**Status**: Pass 1

#pcie/module01

---

## One-liner

> PCIe replaces PCI's shared parallel bus with serial point-to-point links and a packet-based layered protocol, while keeping software concepts like configuration space and BARs for easier migration.
---

## Pass 1 — Outline (watch 1×)

| Time | Topic              | Notes                        |
| ---- | ------------------ | ---------------------------- |
| 0:00 | Intro / Agenda     |                              |
|      | Early buses -> PCI | ISA, unified I/O             |
|      | PCI limits         | Shared bus, pins, clock      |
|      | PCI-X (optional)   | P2P mode, still parallel     |
|      | PCIe value         | Serial, P2P, packets, layers |
|      | 3-layer model      | TLP/DLLP/Physical            |
| end  | preview M2         | Topology next                |

### Checklist (from study plan)

- [ ] PCIe history, comparison with PCI
- [ ] Layered protocol architecture

---

## Key concepts

### PCIe history & motivation

Four PCI bottlenecks:

| Problem               | Meaning                                                |
| --------------------- | ------------------------------------------------------ |
| Shared bus            | all devices share one bus; bandwidth is contested      |
| Parallel + high clock | many wires, skew, reflections; hard to raise frequency |
| Pin / cost            | 32/64 data lines + address/control                     |
| Poor scalability      | more devices = more bus load                           |
Typical PCI peaks : 32-bit @ 33MHz = 133 MB/s ; 64-bit @ 66MHz = 533 MB/s.

### PCIe vs PCI (comparison table)

| Aspect          | PCI                 | PCIe                            |
| --------------- | ------------------- | ------------------------------- |
| Topology        | shared bus          | point-to-point links + switch   |
| Signaling       | parallel            | serial differential             |
| Bandwidth scale | 133-533 MB/s shared | per-link Gen x lanes, no shared |
| Arbitration     | Bus arbitration     | no global arbitration           |
| Hot-plug / PM   | Limited             | Built-in                        |

### Layered protocol architecture

- **Transaction Layer** — TLPs; what the "user" sees
- **Data Link Layer** — DLLPs; reliable delivery on a link
- **Physical Layer** — symbols, lanes, LTSSM

```
[Software / Driver]
        |
   Transaction Layer  (TLP)
        |
   Data Link Layer    (DLLP, ACK/NAK)
        |
   Physical Layer     (Gen1/2/3…, lanes)
        |
    ===== link =====
```

*Later modules drill into each layer — link forward:* [[M05_tlp_elements]], [[M09_dllp_acknak]], [[M13_ltssm]]

---

## Diagrams

*Module 1 多为概念图；复杂图可画到 `diagrams/M01_layers.svg` 再嵌入。*

---

## Cross-reference

### PCILeech / RTL

| Topic | File | Notes |
|-------|------|-------|
| N/A (intro) | — | Module 1 无代码对照；建立心理模型即可 |

### Research

- [[Thunderclap_NDSS2019]] — 攻击发生在 DMA/TLP 层；本模块是栈底上下文
- 检测项目 — 监控对象 eventual 是 **TLP**（Transaction Layer）

### Related modules

- Next: [[M02a_overview]] — topology (Root Complex, Switch, Endpoint)

---

## Questions / unclear

1. 
2. 

---

## Blog outline

**Working title**: PCIe: Why it Exists and Why You Should Care

1. PCI 的瓶颈（个人电脑 / 服务器语境）
2. 串行 + 点对点如何解决
3. 三层模型预览（不展开 TLP）
4. 预告：Config space、TLP、PCILeech

---

## Completion log

| Step | Done | Date |
|------|------|------|
| Watch 1× | [ ] | 2026-07-09 |
| Notes Pass 2 | [ ] | |
| Re-watch hard parts | [ ] | |
| PCILeech cross-ref | [ ] | n/a |
| Blog draft | [ ] | |
