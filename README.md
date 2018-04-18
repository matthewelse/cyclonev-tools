# cyclonev-tools
Useful stuff for building projects with the Altera Cyclone-V SoC

## Building a Debian SD Card Image

Since the provided SD card images for the DE1-SoC are really old, or have unnecessary features, we can build a Debian disk 
image from scratch relatively easily:

Essentially, we need three components:

- U-boot bootloader
- Linux Kernel
- Debian Root Filesystem

Note that the root filesystem doesn't actually depend on the particular board we're using - as long as we're using an ARMv7HF
compatible root filesystem, we can do what we like.

> Instructions are based on notes from here: https://github.com/matthewelse/CycloneVSoC-examples/tree/master/SD-operating-system/Angstrom-v2016.12 and here: https://blog.night-shade.org.uk/2013/12/building-a-pure-debian-armhf-rootfs/, but updated for the latest available versions of Linux and Debian.
> Thanks to the orginal authors of these articles, and I don't claim credit for their work - I'm aiming to simply showing how 
> these efforts can be combined. :)

*Tested with an Ubuntu 17.10 build machine - note that Quartus doesn't play nicely with the Wayland display server, so when you 
log into Ubuntu, you might need to click the 'with X.org' option*

### Step 1: Build the Golden Hardware Reference Design

Start by building the Golden Hardware Reference Design (GHRD), found in the DE1-SoC CD ROM. This gives you the default FPGA 
image that will be loaded when the CPU boots (note that this can be replaced after booting with your own code using the JTAG 
programmer). 

Once this is synthesized, convert the SOF file to an RBF file using the 'Convert Programming Files' tool in Quartus. Pick the `Passive Parallel x16` option when converting, and save the resulting file somewhere you can remember for later.

### Step 2: Build U-Boot

We'll start by generating the device tree for U-Boot. Note that the exact commands below may vary depending on the software 
version used and your exact install location:

```bash
$ ~/intelFPGA/17.1/embedded/embedded_command_shell.sh
$ bsp-editor &
```

Create a new HPS BSP, choose the generated handoff directory from building the GHRD (probably something like 
`de1_soc_GHRD/hps_iws_handoff/soc_system_hps_0`), which will automatically populate most of the settings.

Make sure that `BOOT_FROM_SDMMC` and `FAT_SUPPORT` are checked on the right hand side of the screen. `FAT_BOOT_PARTITION`
should be set to 1, and `FAT_LOAD_PAYLOAD_NAME` should be set to `u-boot.img`.

Under `Advanced/spl/performance`, make sure that `SERIAL_SUPPORT` is checked.







