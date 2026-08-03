# M02a: PCIe Overview Part 1

**Source**: MindShare PCIe 3.1 — Module 2a (Overview Part 1, ~105 MB)  
**Plan**: `PCIe_project/01_PCIe_and_OS_Study_Plan.md` Module 2a  
**Date started**:  
**Date completed**:  
**Time spent**: __ h  
**Status**: Watch ✅ · Cross-ref read ✅ · Notes **v1 uploaded ✅** (2026-07-22)

#pcie/module02a

---

## One-liner

PCIe is a tree topology rooted at the Root Complex. Devices connect via Links made of Lanes. A port is the logical interface on a device. An endpoint is a leaf device (including an FPGA DMA board). Unlike PCI's shared bus, PCIe uses point-to-point serial links.

---

## Pass 1 — Outline (watch 1×)

| Topic                          | Notes (≤1 line)                                |
| ------------------------------ | ---------------------------------------------- |
| PCIe system diagram            | tree, not shared bus                           |
| Root Complex                   | host side; initiates transactions, enumeration |
| PCIe Switch                    | 1 upstream + N downstream ports                |
| Endpoint                       | Leaf device; no downstream PCIe ports          |
| Bus numbering                  | logical bus numbers after enumeration          |
| Lane                           | one RX/TX differential pair                    |
| Link                           | connection between two ports; width + speed    |
| Port                           | logical connection point on a device           |
| Link width x1, x2, x4, x8, x16 | number of parallel lanes                       |
| slot vs connector              | physical slot vs logical link                  |
| device discovery/enumeration   | config reads after reset                       |
| PCIe vs PCI topology           | point-to-point vs shared bus                   |

### Checklist (from study plan)

- [ ] Notes: System topology (Root Complex, Switch, Endpoint)
- [ ] Notes: Lanes, links, ports
- [x] Cross-ref read: PCILeech as Endpoint (`vmd_top.sv` + README-EN)
- [ ] Cross-ref summarized in section below

---

## Key concepts

### System topology

**Serial Transport v.s. Parallel transport**
1. the need of speed: it may only send one bit at a time.
2. parallel transport should overcome problems: 
	1. clock skew (detracts from timing budget but never be eliminated).
	2. signal skew.
	3. flight time.
3. Serial transport:
	1. flight time becomes a non-issue (because the clock latching the data into the receiver is built into the data stream and no external reference clock is necessary).
	2. no clock skew.
	3. no signal skew in on Lane (in different Lanes, it still has signal skew).

**Bandwidth**
	Gen1: 8b/10b, 2.5GT/s.
	Gen2: 8b/10b, 5GT/s.
	Gen3: 128b/130b, 8GT/s.


**Root Complex (RC)**
- Role: the interface between the system CPU and the PCIe topology, with PCIe ports labeled as "Root Ports" in configuration space.
- Relation to CPU / memory / IOMMU:
	- on the host (CPU) side; root of the PCIe hierarchy.
	- integrates or sits next to memory controller, CPU; often IOMMU for DMA remapping.
	- initiates most transactions (CPU access to devices, enumeration).
	- has Root Port(s) downward to Switch or Endpoint.
	- OS view: start of PCI tree.

**Switch and Bridges**
- PCIe Switch:
	- 1 upstream port (toward RC) + multiple downstream ports (toward EPs or other Switches).
	- expands PCIe port count when the motherboard runs out of slots.
	- routes TLPs from upstream to the correct downstream port.
- PCIe Bridge:
	- forward bridge: allows an older PCI or PCI-X card to be plugged into a new system.
	- reverse bridge: allows a new PCIe card to be plugged into an old PCI system.

**Endpoint (EP)**
- leaf of the tree (downstream of a Switch or Root Complex).
- normally no downstream ports for other PCIe devices.
- examples: GPU, NVMe, NIC, FPGA DMA board.
- type 0 header: normal Endpoint.
- type 1 header: birdge-like (Switch, PCIe-to-PCIe bridge).
- legacy PCIe Endpoints: PCI or PCI-X endpoint.
- Native PCIe Endpoints.

```
              [ CPU + Memory ]
                     |
              [ Root Complex ]
               Root Port(s)
                     | Link
        +------------+------------+
        |                         |
   [ Switch ]                  [ EP: NVMe ]
up |    | down                   ×4 common
   |    +--- Link ×16 --- [ GPU ]
   |
   +--- Link ×1 --- [ PCILeech FPGA EP ]
```

### Lane, Link, Port

| Term | Definition                                                                                                                                                                              | My notes                               |
| ---- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------- |
| Lane | It consists of a serial send/receive path made up of four wires (2 each for differential Tx and Rx).                                                                                    | TX/RX<br>x1, x2, x4, x8, x12, x16, x32 |
| Link | It can have a minimum of 1 Lane or as many as 32 Lanes, and number of Lanes in a Link is called the link width. A link connects two devices.                                            |                                        |
| Port | logic port of PCIe. Root Complex at least has one root port. Switch should have upstream and downstream ports. Endpoint usually has one port which is used to be connected to one link. |                                        |

- Common link widths (×1, ×4, ×8, ×16):
- How width relates to bandwidth (→ see [[M02b_overview]]):

| device                | typical link width |
| --------------------- | ------------------ |
| PCILeech / small FPGA | x1                 |
| NVMe SSD              | x4                 |
| GPU                   | x8 or x16          |
| Chipset internal      | wider              |


### Enumeration (brief)
- Goal: RC/firmware/OS discovers devices and assigns resources.
- Mechanism: Configuration Space access (Config Read/Write TLPs) - Module 3.
- Rough sequence:
	- reset de-assert (PERST#).
	- link training -> link up.
	- config reads from bus 0 (Vendor ID / Device ID).
	- assign BDF (Bus / Device / Function).
	- parse BARs, assign memory/IO space.
	- load driver.
- BDF (Bus / Device / Function):
	- 8-bit Bus + 5-bit Device + 3-bit Function; used in routing and TLP Requester / Completer ID.

### Preview: Memory Read TLP (full detail in Module 5)

RC / CPU often starts a Memory Read (MRd) toward an Endpoint BAR. Header only — data comes back in CplD.

![[diagrams/MRd_tlp.svg]]

*Source file: `diagrams/MRd_tlp.svg` — edit in Obsidian (open SVG) or any text/vector editor.*

---

## Spec & terminology

| Term                  | Meaning                        | Notes                                                    |
| --------------------- | ------------------------------ | -------------------------------------------------------- |
| RC                    | Root Complex                   | host-side root                                           |
| EP                    | Endpoint                       | leaf device                                              |
| BDF                   | Bus : Device : Function        | eg. 8'h12 : 5'b00100 : 3'b001                            |
| Upstream / Downstream | towards RC / away from RC      | switch up port / switch down port                        |
| VMD                   | Intel Volume Management Device | PCILeech disguise — see Cross-ref                        |
| Root Port             | RC downstream port             |                                                          |
| FT601                 | USB3 FIFO chip                 | PCILeech control plane (not PCIe spec)                   |
| stealth               | disguise / hide                | fake IDs, BAR behavior, etc.                             |
| DMA                   | Direct Memory Access           | EP reads / writes host memory                            |
| IOMMU                 | IO MMU                         | limits device physical access; Thunderclap bypass topic. |

---

## Cross-reference

### PCILeech / RTL — Endpoint topology

**FPGA project**: `PCILeech-FPGA-DMA_VMD-main/.../src/pcileech_75t484_x1_vmd_top.sv`

| Topic in M02a | File / signal | What to look for |
|---------------|---------------|------------------|
| PCILeech = Endpoint | `vmd_top.sv` | |
| Link ×1 | `pcie_tx_p[0:0]`, `pcie_rx_p[0:0]` | |
| Board / chip | README-EN.md | |
| VMD disguise | `PARAM_VENDOR_ID`, `PARAM_DEVICE_ID`, `PARAM_CLASS_CODE` | |
| USB control path | `pcileech_com`, FT601 | |
| TLP / DMA path | `pcileech_fifo` → `pcileech_pcie_a7` | |
| Link status | `pcie_lnk_up`, `pcie_perst_n` | |
| BDF at runtime | `cfg_bus_number`, `cfg_device_number`, `cfg_function_number` | |

**Data flow (sketch)**

```
（PC → USB → com → fifo → pcie_a7 → RC → DRAM）
```


**VMD (one sentence)**

- 

**Config space preview** (Module 3 deep-dive)

| Topic | File | Notes |
|-------|------|-------|
| Vendor/Device ID in COE | `ip/pcileech_cfgspace.coe` | |

### Research / papers

- [[Thunderclap_NDSS2019]] —
- Detection project —

### Related modules

- Prerequisite: [[M01_background]]
- Next: [[M02b_overview]]
- Then: [[M03_config_space]] *(create when Module 3 starts)*

---

## Questions / unclear

1. 
2. 

*Re-watch timestamps:*

---

## Blog outline

**Working title**: PCIe Topology Explained with Real Examples

1. Hook:
2. 
3. 
4. PCILeech as Endpoint example:
5. Takeaway:

---

## Completion log

| Step | Done | Date |
|------|------|------|
| Watch 1× | [x] | 2026-07-13 |
| Notes Pass 2 | [ ] | |
| Re-watch hard parts | [ ] | |
| PCILeech cross-ref read | [x] | 2026-07-13 |
| PCILeech cross-ref in notes | [ ] | |
| Blog draft | [ ] | |
