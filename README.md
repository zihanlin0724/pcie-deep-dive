# pcie-deep-dive

> A systematic study of PCI Express Gen3 protocol — from spec reading
> to RTL implementation, based on MindShare PCIe 3.1 materials and
> the open-source PCILeech-FPGA project.

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Language](https://img.shields.io/badge/Language-SystemVerilog%20%7C%20Python-blue.svg)](#)
[![Status](https://img.shields.io/badge/Status-Active-brightgreen.svg)](#)

---

## About This Repository

I am a digital design and verification engineer with 5+ years of
industry experience, currently preparing for a PhD application in
hardware security and computer architecture. This repository
documents my deep-dive into the PCIe 3.x protocol, with a focus
on understanding the PCILeech FPGA DMA attack framework.

The repository contains:

- **23 study notes** corresponding to MindShare PCIe 3.1 modules
- **23 technical blog posts** explaining each topic with diagrams
- **4 RTL mini-projects** implementing core PCIe building blocks
- **Code walkthroughs** of the PCILeech-FPGA-DMA project
- **Paper summaries** of relevant academic work on PCIe security

---

## Repository Structure

```
pcie-deep-dive/
├── notes/        # 23 module study notes (markdown)
├── blogs/        # 23 published technical blogs
├── rtl/          # 4 RTL implementation projects (SystemVerilog)
├── diagrams/     # Self-drawn protocol diagrams (SVG)
├── papers/       # Paper summaries and reading list
└── docs/         # Cross-module documents (glossary, roadmap)
```

---

## Progress Tracker

### MindShare Module Coverage

| # | Module | Notes | Blog | RTL |
|---|--------|-------|------|-----|
| 1 | PCIe Background Info | TODO | TODO | - |
| 2 | PCIe Overview | TODO | TODO | - |
| 3 | Configuration Space | TODO | TODO | - |
| 4 | Address Space & Routing | TODO | TODO | - |
| 5 | TLP Elements | TODO | TODO | TODO |
| 6 | Flow Control | TODO | TODO | TODO |
| 7 | Quality of Service | TODO | TODO | - |
| 8 | Transaction Ordering | TODO | TODO | - |
| 9 | DLLP & AckNak | TODO | TODO | TODO |
| 10 | Physical Layer (2.5/5.0GT) | TODO | TODO | - |
| 11 | Physical Layer (8.0GT) | TODO | TODO | - |
| 12 | Physical Layer Electrical | TODO | TODO | - |
| 13 | Link Training (LTSSM) | TODO | TODO | TODO |
| 14 | Interrupts (MSI/MSI-X) | TODO | TODO | - |
| 15 | Error Detection | TODO | TODO | - |
| 16 | Power Management | TODO | TODO | - |
| 17 | System Resets | TODO | TODO | - |
| 18 | Hot Plug | TODO | TODO | - |
| 19 | Spec 2.1 Overview | TODO | TODO | - |
| 20 | Spec 3.1 Overview | TODO | TODO | - |
| 21 | Spec 2.1 Details | TODO | TODO | - |
| 22 | Spec 3.1 Details | TODO | TODO | - |
| 23 | Arbor Lab | TODO | TODO | - |

---

## RTL Mini-Projects

### 1. TLP Encoder / Decoder (`rtl/tlp_codec/`)

A SystemVerilog implementation of PCIe Gen3 TLP header
encoding and decoding, supporting Memory Read/Write, Config
Read/Write, and Completion TLPs.

- Status: Planned
- Target: 100% feature coverage with directed and constrained-random tests

### 2. Link Training State Machine (`rtl/ltssm/`)

A simplified implementation of the PCIe LTSSM covering Detect,
Polling, Configuration, L0, L0s, L1, and Recovery states.

- Status: Planned
- Reference: PCIe 3.1 spec Chapter 4.2.5

### 3. DLLP Ack/Nak Engine (`rtl/dllp_acknak/`)

Implements the Data Link Layer's reliability mechanism with
sequence numbering, replay buffer, and ACK/NAK generation.

- Status: Planned

### 4. Flow Control Manager (`rtl/flow_control/`)

Credit-based flow control with separate P / NP / Cpl credit
pools and 8-VC support.

- Status: Planned

---

## Related Reading

A curated list of papers I have read or plan to read on PCIe,
DMA attacks, and hardware security. See [`papers/reading_list.md`](papers/reading_list.md).

Highlights:

- **Thunderclap** (NDSS 2019) — DMA attacks via Thunderbolt 3
- **PCILeech / DMA Lookout** — Open-source FPGA DMA framework
- **CXL Survey** — The evolution of PCIe into CXL for compute-attached memory

---

## My Independent Research

This study is part of a larger research project I am working on:

> **Detecting FPGA-Based DMA Attacks via PCIe TLP Pattern Analysis**

Project repository: [pcie-dma-attack-detector](https://github.com/zihanlin0724/pcie-dma-attack-detector) (work in progress)

Goal: Identify PCILeech-style attacks at the TLP level using
machine learning and a lightweight hardware monitor.

---

## About Me

- **Name**: Zihan Lin
- **Background**: M.S. EE, University of Southern California (2019)
- **Industry**: 5+ years RTL design and DV (Sophoton, Wipro, Sprintek)
- **Specialties**: LPDDR5, PCIe, AXI, high-speed CDC
- **Looking for**: PhD program in hardware security / computer architecture (Fall 2027)

Contact: edwardzihanlin@gmail.com | [LinkedIn](https://linkedin.com/in/zihanlin0724)

---

## License

This repository is released under the [MIT License](LICENSE).

The MindShare videos and PCI System Architecture book referenced
in my notes are **not redistributed** — only my own notes,
diagrams, and summaries are included here.
