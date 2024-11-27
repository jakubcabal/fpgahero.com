---
title: "100/400 Gigabit Ethernet overview"
date: "2024-11-25"
author: "Jakub Cabal"
tags: 
  - "Ethernet"
  - "Gigabit Ethernet"
  - "400G"
  - "Hard IP"
  - "Soft IP"
  - "QSFP"
  - "QSFP-DD"
  - "transceivers"
  - "SERDES"
---

Ethernet is the technology that allows us to connect individual devices to networks and the entire Internet. 
Different devices use Ethernet for communication, remote control and also for data transfer.
Also, FPGA-based applications use Ethernet very often, and newer FPGAs have Ethernet interface support directly built in in the form of hard IP blocks.

The need to transfer larger and larger volumes of data is constantly growing every year.
For that reason, even Ethernet technology is constantly evolving, and the magical 1 Terabit threshold is just around the corner.
At the same time, it was not so long ago when the first 100 Gigabit FPGA card appeared in 2013.
Relatively recently, Intel/Altera introduced its Agilex 7 I-Series FPGA chip with even 400 Gigabit Ethernet support.

## How to connect 100/400 Gigabit Ethernet to FPGA?

Over longer distances, 100/400 Gigabit Ethernet is spread exclusively through optical cables.
Metallic cables can also be used for distances in the order of meters.
So-called transceivers are used to convert optical signals into electrical signals.

Transceivers in QSFP28 (Quad Small Form-factor Pluggable, 4x 28 Gbps) format are almost exclusively used for 100 Gigabit Ethernet.
For 400 Gigabit Ethernet, we most often encounter the QSFP-DD format (Quad Small Form-factor Pluggable - Double Density, 8x 56 Gbps),
however, it is possible that the QSFP112 (4x 112 Gbps) format will replace it over time.

The transceiver format defacto defines the electrical connection of the Ethernet to the FPGA chip.
Here, extremely fast serial lines are typically used for both directions of communication (RX and TX).
Today's most advanced FPGAs can communicate over one such serial line at speeds up to 112 Gbps.

The SERDES block in the FPGA converts the serial line to parallel.
Next is the Ethernet Hard IP (or Soft IP), where the logic to decode the Ethernet protocol is implemented.
The Ethernet IP can only implement the PCS layer, where [Media-Independent Interface (MII)](https://en.wikipedia.org/wiki/Media-independent_interface) is used as the user interface.
More often, there is also a MAC layer (PCS+MAC) implemented.
It performs CRC calculation and checking, statistics computation and many other functions.
The user interface of the MAC layer is a standard streaming interface (for example, Avalon-ST) that allows the transmission of Ethernet frames.
Ethernet IP often includes support for RS FEC (Reed-Solomon error correction), which is mandatory for some Ethernet standards.

### There is no Ethernet like Ethernet- TODO

Under the word Ethernet there are also many standards that differ not only in transmission speed.
Some standards are designed for metallic cables, others for fiber optics.
For example, 100 Gigabit Ethernet can be transmitted over four serial lines or just two lines.
The difference may also be that some standards require RS-FEC (Reed–Solomon error correction, there are different variants) and others do not.

There are a lot of these Ethernet standards,
for an example you can look at the [list of just those for 100 Gbps transmission speeds](https://en.wikipedia.org/wiki/100_Gigabit_Ethernet#100G_interface_types).
The engineer should know exactly what he will be working with, but it is not easy to know.
Fortunately, there are only a few widely used Ethernet standards and we don't see many others in practice.

## 100/400 Gigabit Ethernet Hard IPs - TODO

Among FPGA chip manufacturers, AMD (Xilinx) and Altera (Intel) are the two main companies dominating.
Both manufacturers now have 100 Gigabit Ethernet-enabled FPGAs in their lineup and
occasionally we can also find FPGAs that can even handle 400 Gigabit Ethernet.

In the area of large FPGAs, Achronix is trying to get between the two manufacturers with its [Speedster7t](https://www.achronix.com/product/speedster7t-fpgas) chip,
which also has support for up to 400 Gigabit Ethernet.
However, its documentation is very weak so far and so I will focus on the two main manufacturers for now.

Let's conclude this post with a table comparing the available 100+ Gigabit Ethernet Hard IPs:

| Vendor | Hard IP name |  FPGA family    | Supported speeds | RS-FEC | User interface |
| ------ | ------- | -------------------- | ------------ |------- | ------------- |
| AMD    | CMAC    | UltraScale(+)        | 100 Gb only  | YES*   | LBUS, AXI4-ST |
| AMD    | MRMAC   | Versal AI Core/HBM/Premium | 10 to 100 Gb | YES    | AXI4-ST (Segmented) |
| AMD    | DCMAC   | Versal HBM/Premium   | 100, 200, 400 Gb | YES    | AXI4-ST (Segmented) |
| Altera | H-Tile  | Stratix 10           | 100 Gb only  | NO    | Avalon-ST     |
| Altera | E-Tile  | Stratix 10, Agilex 7 | 10, 25, 100 Gb | YES    | Avalon-ST     |
| Altera | F-Tile  | Agilex 7             | 10 to 400 Gb | YES    | Avalon-ST, MAC Segmented |

**Beware, some FPGAs even from the same family may have Ethernet or other Hard IP block support cut.**
For example, not every Stratix 10 FPGA with H-Tile has a built-in Hard IP block for Ethernet.
You should always read the manufacturer's documentation carefully.

## How to achieve full throughput - TODO

## Open-source FPGA projects with 100/400 Gigabit Ethernet support - TODO

----

**Sources:**

- [UltraScale+ Devices Integrated 100G Ethernet Subsystem LogiCORE IP Product Guide (PG203)](https://docs.amd.com/r/en-US/pg203-cmac-usplus)
- [Versal Devices Integrated 100G Multirate Ethernet MAC Subsystem Product Guide (PG314)](https://docs.amd.com/r/en-US/pg314-versal-mrmac)
- [Versal Adaptive SoC 600G Channelized Multirate Ethernet Subsystem (DCMAC) LogiCORE IP Product Guide (PG369)](https://docs.amd.com/r/en-US/pg369-dcmac)
- [H-Tile Hard IP Ethernet Intel FPGA IP User Guide](https://www.intel.com/content/www/us/en/docs/programmable/683430/24-1-19-6-0/about-the-ip-core.html)
- [E-Tile Hard IP User Guide](https://www.intel.com/content/www/us/en/docs/programmable/683468/23-2/about-e-tile-hard-ip-user-guide.html)
- [F-Tile Ethernet Intel FPGA Hard IP User Guide](https://www.intel.com/content/www/us/en/docs/programmable/683023/24-3/overview-16832.html)
