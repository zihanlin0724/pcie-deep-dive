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

A TLP is the PCIe transaction (header ± data ± digest); the first-DW **Fmt+Type** field picks 3DW vs 4DW and MRd/MWr/Cfg/Cpl — that is how PCILeech injects DMA, and how a detector tells an attacker from a real device.

---

## Pass 1 — Outline (watch 1×)

| Topic | Notes (≤1 line) |
| ----- | --------------- |
| Why packet-based protocol | split a transaction into packets so each hop can switch independently (not a shared bus cycle) |
| TLP assembly / disassembly | Transaction Layer builds/parses TLP; Data Link adds Seq# + LCRC; PHY serializes |
| Generic TLP structure (Hdr + Data + Digest) | Header always; Data payload optional; Digest/ECRC optional last DW |
| Fmt + Type field | first DW: Fmt = 3DW/4DW ± data; Type = MRd/MWr/Cfg/Cpl/Msg — decode key for PCILeech |
| 3DW vs 4DW header | 3DW = 32-bit addr or ID-routed (Cfg/Cpl); 4DW = 64-bit memory address |
| Memory Read / Memory Write | MRd non-posted, no data; MWr posted, has data; must not cross a 4KB boundary |
| I/O Read / Write | legacy IO map (often only 64KB); always 3DW; non-posted |
| Config Read / Write | always 3DW, ID-routed (Bus/Dev/Func); Type 0 CfgWr captures Bus/Device as later Requester ID |
| Completion (Cpl / CplD) | Cpl = status only; CplD = status + data; matches Tag + Requester ID of the request |
| Message TLPs | ID- or implicitly routed; interrupts, errors, PM; Msg vs MsgD (± data) |
| First/Last DW BE (byte enables) | which bytes in the first/last DW are valid; Length is in DWs, BE trims the edges |
| Tag / Requester ID / Completer ID | RID + Tag uniquely ID a non-posted request; Completer ID is who sent the Cpl |
| Traffic Class / Attr / Length | TC = QoS class (VC mapping); Attr = RO / No Snoop / ID-based ordering; Length = payload DWs |
| ECRC / Digest (brief) | optional end-to-end CRC on the TLP; Digest bit in header means ECRC DW is present |
| Posted vs Non-Posted | Posted (MWr/Msg) = no completion; Non-posted (MRd/IO/Cfg) = must get Cpl/CplD or timeout |

### Checklist (from study plan)

- [x] Notes: TLP Format (Fmt + Type fields)
- [x] Notes: TLP Header layout (3DW vs 4DW)
- [x] Notes: Memory Read, Memory Write TLPs
- [x] Notes: I/O Read/Write, Config Read/Write
- [x] Notes: Completion (Cpl, CplD) TLPs
- [x] Notes: Message TLPs
- [x] Notes: First/Last DW BE (byte enables)
- [x] Notes: Tag, Requester ID, Completer ID
- [ ] Cross-ref: `pcileech_pcie_tlp_a7.sv`
- [ ] Cross-ref: `pcileech_tlps128_bar_controller.sv`
- [x] eBook Ch 5: deep read
- [ ] Blog: "PCIe TLP Format Decoded: Reading Hex Like a Pro"

---

## Key concepts

### Generic TLP structure
	header + payload (optional) + ECRC (optional)


### Fmt + Type

|                 TLP                  |                 FMT[2:0]                 | TYPE[4:0] |
| :----------------------------------: | :--------------------------------------: | :-------: |
|      Memory Read Request (MRd)       | 000 = 3DW, no data<br>001 = 4DW, no data |   00000   |
|   Memory Read Lock Request (MRdLk)   | 000 = 3DW, no data<br>001 = 4DW, no data |   00001   |
|      Memory Write Request (MWr)      | 010 = 3DW, w/ data<br>011 = 4DW, w/ data |   00000   |
|        IO Read Request (IORd)        |            000 = 3DW, no data            |   00010   |
|       IO Write Request (IOWr)        |            010 = 3DW, w/ data            |   00010   |
| Config Type 0 Read Request (CfgRd0)  |            000 = 3DW, no data            |   00100   |
| Config Type 0 Write Request (CfgWr0) |            010 = 3DW, w/ data            |   00100   |
| Config Type 1 Read Request (CfgRd1)  |            000 = 3DW, no data            |   00101   |
| Config Type 1 Write Request (CfgWr1) |            010 = 3DW, w/ data            |   00101   |
|        Message Request (Msg)         |            001 = 4DW, no data            |   10rrr   |
|    Message Request W/Data (MsgD)     |            011 = 4DW, w/ data            |   10rrr   |
|           Completion (Cpl)           |            000 = 3DW, no data            |   01010   |
|       Completion W/Data (CplD)       |            010 = 3DW, w/ data            |   01010   |
|      Completion-locked (CplLk)       |            000 = 3DW, no data            |   01011   |
|  Completion W/Data Locked (CplDLk)   |            010 = 3DW, w/ data            |   01011   |
TBD


### Memory / I/O / Config / Completion / Message

| TLP       | Post?      | Data?              |
| --------- | ---------- | ------------------ |
| MRd       | Non-posted | no                 |
| MWr       | Post       | with               |
| I/O Rd/Wr | Non-posted | Rd-no / Wr-with    |
| Cfg Rd/Wr | Non-posted | Rd-no / Wr-with    |
| Cpl       | Non-posted | no                 |
| Cpld      | Non-posted | with               |
| Msg/MsgD  | Post       | Msg-no / MsgD-with |

#### 3DW and 4DW Headers
![[diagrams/tlp_header.drawio]]

#### IO Request Notes
1. Allowance of IO transactions is made for Legacy devices and may need to rely on a compatible device residing in the system IO map rather than the memory map.
2. While the IO transactions can technically access a 32-bit IO range, in reality many systems (and CPUs) restrict IO access to the lower 16 bits (64KB) of this range.
#### Memory Request Notes
1. Due to data payload size is between 0 and 1024 DW (Length[9:0]), memory data transfers are not permitted to cross a 4KB boundary.
2. All memory-mapped writes are posted to improve performance.
3. Either 32- or 64-bit addressing may be used.
4. Including up to 8 Traffic Classes, quality of service features may be used.
5. The No Snoop attribute can be used to relieve the system of the need to snoop processor caches when transactions target main memory.
6. The Relaxed Ordering attribute may be used to allow devices in the packet's path to apply the relaxed ordering rules in hopes of improving performance.
#### Configuration Request Notes
1. Configuration requests always use the 3DW header format and are routed based on the target Bus, Device, and Function numbers.
2. All devices "capture" their Bus and Device number from the target numbers in the Request whenever they receive a Type 0 configuration write cycle. The reason for that is because they will need it later to use as their Requester ID when they send requests of their own in the future.
#### Completion Notes
1. Summary of Completion Status Codes
	- 000 : Successful Completion (SC).
	- 001 : Unsupported Request (UR).
	- 010 : Configuration Request Retry Status (CRS) - Completer is temporarily unable to service a configuration request, and the request should be attempted again later.
	- 100 : Completer Abort (CA) - Completer should have been able to service the request but has failed for some reason. This is an uncorrectable error.
2. For Memory Read Requests, Lower Address Field is an offset from the DW start address: 0, +1, +2, +3. For AtomicOp Completions, the Lower Address field is reserved. For all other Completion types, it is set to zero.
3. The Byte Count Modified Bit (BCM)
	- It is only set by a PCI-X Completer if a read request is going to be broken into multiple completions.
	- It is only set for the first Completion of the series.
	- For the subsequent Completions in the series, the BCM bit must be de-asserted and Byte Count field will reflect the total remaining count as it normally would.
	- Devices receiving Completions with the BCM bit set must interpret this case properly.
	- The Lower Address field is set by the Completer during completions with data to reflect the address of the first enabled byte of data being returned.
4. Data Returned for Read Requests
	- A read request may require multiple completions to be fulfilled, but total data transfer must eventually equal the size of original request, or a Completion Timeout error will probably result.
	- A given Completion can only service on Request.
	- IO and Configuration reads are always 1DW, and will always be satisfied with a single Completion.
	- A Completion with a Status Code other than SC (successful) terminates a transaction.
	- The Read Completion Boundary (RCB) must be observed when handling a read request with multiple completions. The RCB is 64 bytes or 128 bytes for the Root Complex, since it is allowed to modify the size of packets flowing between its ports, and the value used is visible in a configuration register.
	- Bridges and endpoints may implement a bit for selecting the RCB size (64 or 128 bytes) under software control.
	- Completions that are entirely within an aligned RCB boundary must complete in one transfer, since the transfer won't reach the RCB, which is the only place it can legally stop early.
	- Multiple Completions for a single read request must return data in increasing address order.
5. Receiver Completion Handling Rules
	- A received Completion that does not match a pending request is an Unexpected Completion and treated as an error.
	- Completions with a completion status other than SC or CRS will be handled as errors and buffer space associated with them will be released.
	- When the Root Complex receives a CRS status during a configuration cycle, the request is terminated.
		- If CRS Software Visibility bit is not enabled, the Root will reissue the config request for an implementation-specific number of times before giving up and concluding the target has a problem.
		- If CRS Software Visibility bit is enabled, software designed to support it will always read both bytes of the Vendor ID field first. It returns the value of 0001h for Vendor ID.
	- A CRS status in response to a request other than configuration is illegal and may be reported as a Malformed TLP.
	- Completions with status which is reserved code are treated as Unsupported Request (UR).
	- If a Read Completion or an AtomicOp Completion is received with a status other than SC, no data is included with the completion and the Requester must consider this Request terminated. How the Requester handles this case is implementation-specific.
#### Message Requests Note
	TBD


### Byte enables (First/Last DW BE)
1. Last DW Byte Enables[3:0] : These four high-true bits map one-to-one to the bytes within the last double word of payload.
2. First DW Byte Enables[3:0] : These four high-true bits map one-to-one to the bytes within the first double word of payload.

### IDs and Tag

Matching key for non-posted traffic: **(Requester ID, Tag)**. Completions copy both. Posted TLPs (MWr / Msg) still carry RID + Tag in the header, but software/hardware does **not** wait for a Cpl, so Tag is not used for matching.

| Field | Role | Notes |
| ----- | ---- | ----- |
| Requester ID | 16-bit BDF **Bus[7:0] : Device[4:0] : Function[2:0]** (8:5:3). Names **who originated the Request**. Present on every Request TLP. | Device **captures** Bus+Device from a Type 0 **CfgWr** target ID, then uses that as RID on later MRd/MWr/Cfg/IO. Completions **must echo this RID**. PCILeech FPGA EP: RID = captured BDF of the DMA board — host CplDs are addressed back to it. Detector: a flood of MRd with one RID (the FPGA) vs many legitimate device RIDs. |
| Completer ID | Same 16-bit BDF. Names **who generated the Completion**. Appears on **Cpl / CplD / CplLk / CplDLk only** (not on Requests). | Completer is the function that serviced the request (endpoint, or a bridge completing on behalf). CID does not have to equal the original destination BDF if a switch/bridge completes. Unexpected Completion = Cpl whose (RID, Tag) matches nothing outstanding. Detector: CID that is not the expected host/RC or BAR target can mean spoofed completions (less common than malicious MRd). |
| Tag | Uniquely IDs **one outstanding non-posted request from that Requester**. Default **8 bits** (up to 256 in-flight). If Extended Tag is enabled in Control, **10 bits** (up to 768 or 2048, implementation/spec dependent). Completions **copy Tag** from the Request. | Do not reuse a Tag until the request completes or a Completion Timeout. IO/Cfg are 1 DW → one Cpl; a long MRd may return **several CplD** with the **same** (RID, Tag) until Byte Count is satisfied. Posted Tag is don't-care for matching. PCILeech MRd DMA: many Tags in flight; a TLP monitor can track outstanding `(RID, Tag)` vs returning CplD (length, BCM, RCB). |

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
| Tag | 8-bit (or 10-bit extended) ID of one outstanding non-posted request **per Requester** | Echoed in every Cpl/CplD; pair with RID to match. Not used to match Posted TLPs. |
| RID / CID | Requester ID / Completer ID — 16-bit BDF (8:5:3) | RID on Requests + Completions; CID only on Completions. |
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

| Step                        | Done | Date       |
| --------------------------- | ---- | ---------- |
| Watch 1×                    | [x]  | 2026-07-24 |
| Pass 1 outline              | [x]  | 2026-08-17 |
| Notes Pass 2                | [ ]  |            |
| Re-watch hard parts         | [ ]  |            |
| PCILeech cross-ref read     | [ ]  |            |
| PCILeech cross-ref in notes | [ ]  |            |
| eBook Ch 5                  | [ ]  |            |
| Blog draft                  | [ ]  |            |
| TLP Encoder/Decoder RTL     | [ ]  |            |
