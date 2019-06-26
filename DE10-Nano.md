# DE10-Nano Dev Kit SoC

Built by [Terasic][terasic]

Mine came from [Mouser][mouser]

I made an [unboxing & overview][youtube] video

## Altera Cyclone V FPGA

The dev kit ships with an Altera SoC, part number 5CSEBA6U23I7NDK.
According to Intel's Cyclone V Device Overview [document][cv51001],
this breaks down as follows:

| Code  | Meaning                                |
| :---: | -------------------------------------- |
| 5C    | Cyclone V product family               |
| SE    | SoC with Enhanced logic/memory         |
| B     | No hard PCIe or hard memory controller |
| A6    | 110,000 Logic Elements                 |
| U     | Ultra FineLine Ball Grid Array         |
| 23    | 672-pin BGA (23 mm)                    |
| I     | Industrial package: -40°C to 100°C     |
| 7     | FPGA fabric speed 7 (6 is fastest)     |
| N     | Lead-free package                      |
| DK    | Developer Kit                          |

### Code A6

"A6" means that the FPGA fabric is built around 41,910 [Adaptive Logic
Modules][alm], which can be converted to 110k LE-equivalents. It also
contains 166,036 registers, 112 variable-precision DSP blocks, and 224
18×18 multipliers.

## ARM Cortex-A9 HPS

In addition to the FPGA, the DE10-Nano SoC contains a hard processor
system (HPS). For all Cyclone V chips, this is a 32-bit ARM Cortex-A9
MPCore containing a pair of cache-coherent ARMV7-A cores using a 28 nm
process. Both cores operate at 800 MHz.

## More Info

Connect to your DE10-Nano's IP address in a Web browser :-)

Intel appears to keep a more up-to-date OS image than Terasic. I would
recommend starting with the [latest image][intel].

<!--References-->

[alm]: https://www.intel.com/content/www/us/en/programmable/quartushelp/current/index.htm?q=/content/www/us/en/programmable/quartushelp/current/reference/glossary/def_alm.htm

[cv51001]: https://www.intel.com/content/dam/altera-www/global/en_US/pdfs/literature/hb/cyclone-v/cv_51001.pdf

[intel]: https://downloadcenter.intel.com/download/26687/Downloads-for-the-Terasic-DE10-Nano-Kit-Featuring-an-Intel-Cyclone-V-FPGA-SoC?v=t

[mouser]: https://www.mouser.com/new/terasic-technologies/terasic-de10-nano-kit/

[terasic]: http://de10-nano.terasic.com

[youtube]: https://youtu.be/g2Q8LKMfIho
