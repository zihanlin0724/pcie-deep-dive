# M01: PCIe Background Info

**Source**: MindShare PCIe 3.1 — Module 1 (Background Info, ~96 MB)  
**Plan**: `PCIe_project/01_PCIe_and_OS_Study_Plan.md` Module 1  
**Date started**: 2026-07-09  
**Date completed**: 2026-07-10 (San Jose)  
**Time spent**: ~3.5 h  
**Status**: Pass 1 + notes complete (blog pending)

#pcie/module01

---

## One-liner

> （例：PCIe 用串行点对点链路替代 PCI 共享总线，并分层实现可扩展的高速 I/O。——看完后再改成你自己的话）

---

## Pass 1 — Outline (watch 1×)

| Time | Topic | Notes |
|------|-------|-------|
| | Why not stay on PCI? | bandwidth, pins, clock |
| | PCIe vs PCI summary | |
| | Layered model intro | Transaction / Data Link / Physical |

### Checklist (from study plan)

- [x] PCIe history, comparison with PCI
- [x] Layered protocol architecture

---

## Key concepts

### PCIe history & motivation

- **Problem PCI had**:
- **What PCIe changed** (serial, point-to-point, packets):

### PCIe vs PCI (comparison table)

| Aspect | PCI | PCIe |
|--------|-----|------|
| Topology | shared bus | point-to-point links |
| Signaling | parallel | serial (embedded clock) |
| Bandwidth scale | | |
| Hot-plug / PM | | |

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
| Watch 1× | [x] | 2026-07-10 |
| Notes Pass 2 | [x] | 2026-07-10 |
| Re-watch hard parts | [x] | 2026-07-10 |
| PCILeech cross-ref | [x] | n/a |
| Blog draft | [ ] | Jul 16–17 |
