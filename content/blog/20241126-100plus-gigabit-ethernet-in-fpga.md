---
title: "100+ Gigabit Ethernet in FPGA"
date: "2024-11-25"
author: "Jakub Cabal"
tags: 
  - "ethernet"
  - "hard-ip"
---

Ethernet is the technology that allows us to connect individual devices to networks and the entire Internet. 
Different devices use Ethernet for communication, remote control and also for data transfer.
It is therefore no coincidence that Ethernet support is also found in a wide range of FPGAs.

Implementing 100 Mbps Ethernet support is not difficult even on the cheapest FPGAs,
but higher speeds may require more sophisticated hardware.

## There is no Ethernet like Ethernet

Under the word Ethernet there are also many standards that differ not only in transmission speed.
Some standards are designed for metallic cables, others for fiber optics.
For example, 100 Gigabit Ethernet can be transmitted over four serial lines or just two lines.
The difference may also be that some standards require RS-FEC (Reed–Solomon error correction, there are different variants) and others do not.

There are a lot of these Ethernet standards,
for an example you can look at the [list of just those for 100 Gbps transmission speeds](https://en.wikipedia.org/wiki/100_Gigabit_Ethernet#100G_interface_types).
The engineer should know exactly what he will be working with, but it is not easy to know.
Fortunately, there are only a few widely used Ethernet standards and we don't see many others in practice.

## Multi Gigabit Ethernet support may vary

Ethernet support in FPGAs can vary not only in transmission speed,
but also the level of built-in support (within the Hard IP blocks).

Some FPGAs have only fast enough serial lines (SERDES or also referred to as transceivers) to support proper encoding (NRZ, PAM4, etc.),
and therefore allow to implement PMA layer Ethernet support.
Better FPGAs have built-in logic to decode the Ethernet protocol at PCS level to the [Media-Independent Interface (MII)](https://en.wikipedia.org/wiki/Media-independent_interface) interface.
However, the main trend is to provide full support, and so the best FPGAs have dedicated logic for all key Ethernet protocol layers including MAC and sometimes optional RS-FEC layer.

**So once again, briefly to summarize:**

- PMA - support only at the high-speed serial lines (SERDES) level
- PMA+PCS - support including decoding to MII interface level
- PMA+PCS+MAC+(RS-FEC) - full Ethernet protocol support

If your FPGA has support for at least the PAM layer, you can implement the remaining ones in the gate array.
Such solutions (in the form of Soft IP) are provided by FPGA chip manufacturers directly or by other companies for a considerable licensing fee.

## Compare 100+ Gigabit Ethernet Hard IPs

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

----

**Sources:**

- [UltraScale+ Devices Integrated 100G Ethernet Subsystem LogiCORE IP Product Guide (PG203)](https://docs.amd.com/r/en-US/pg203-cmac-usplus)
- [Versal Devices Integrated 100G Multirate Ethernet MAC Subsystem Product Guide (PG314)](https://docs.amd.com/r/en-US/pg314-versal-mrmac)
- [Versal Adaptive SoC 600G Channelized Multirate Ethernet Subsystem (DCMAC) LogiCORE IP Product Guide (PG369)](https://docs.amd.com/r/en-US/pg369-dcmac)
- [H-Tile Hard IP Ethernet Intel FPGA IP User Guide](https://www.intel.com/content/www/us/en/docs/programmable/683430/24-1-19-6-0/about-the-ip-core.html)
- [E-Tile Hard IP User Guide](https://www.intel.com/content/www/us/en/docs/programmable/683468/23-2/about-e-tile-hard-ip-user-guide.html)
- [F-Tile Ethernet Intel FPGA Hard IP User Guide](https://www.intel.com/content/www/us/en/docs/programmable/683023/24-3/overview-16832.html)
