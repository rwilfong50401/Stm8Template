# Ray's STM8 Clion Template

## About:
This template is designed for quick startup of STM8 Microcontroller development projects in CLion. It uses current best
practices from JetBrains related to configuration of external compilers. It was designed for development in Linux environment,
but it should work in Windows (WSL?) with tweaks.

## Instructions for use
1. Install each of the **Components** below
2. Copy the **Configuration Files** to their specified locations
3. Change your linked directories in lib/STM8_SPL to point to the corresponding folders of your STM8 Standard Peripheral Library <br>
`rm inc` <br>
`ln -s /PATH/TO/STM8/LIBRARY/inc inc` <br>
`rm src` <br>
`ln -s /PATH/TO/STM8/LIBRARY/src src` <br>
4. Add an external tool with [these settings](./MISC_INSTALL_FILES/stm8flash.png)
5. Set a breakpoint in the code and click the debug button...
5. (Optional) Load the SVD file from MISC_INSTALL_FILES/STM8S103F3.svd

## Components (Required tool-chain elements):
* Small Device C Compiler 4.2.0 [SDCC](https://sdcc.sourceforge.net/)
  * Installed in my Debian environment with apt: `apt install sdcc`
  * Verify install with `sdcc --version`
* STM8 Toolchain with GDB (stm8-gdb) [instructions](https://stm8-binutils-gdb.sourceforge.io/)
  * Verify install with `stm8-gdb --version`
* OpenOCD v0.12.0
  * (web site)[https://openocd.org/pages/getting-openocd.html]
  * (source)[https://sourceforge.net/p/openocd/code/ci/master/tree/]
  * Verify install with `openocd --version`
* stm8flash (because openocd freaks out when uploading) [github source](https://github.com/vdudouyt/stm8flash)
  * Don't forget to read the INSTALL file that tells you to run: `apt-get install pkg-config libusb-1.0-0-dev`
  * Verify install with `stm8flash -V`
* STM8 Standard Peripheral Library 2.3.1 [STSW-STM8069](https://www.st.com/en/embedded-software/stsw-stm8069.html)
  * Patched for SDCC with [STM8-SPL_SDCC_patch](https://github.com/gicking/STM8-SPL_SDCC_patch)

## Configuration Files
* OpenOCD configuration scripts (place in openocd scripts directory)
  * MISC_INSTALL_FILES/openocd/scripts/board/REW_stm8s.cfg
  * MISC_INSTALL_FILES/openocd/scripts/target/REW_stm8s.cfg
  * MISC_INSTALL_FILES/openocd/scripts/target/REW_stm8s103.cfg
* UDEV rules (place in /etc/udev/rules.d directory)
  * MISC_INSTALL_FILES/etc/udev/rules.d/49-stlinkv2-REW.rules

## How to configure a new project ##
1. Copy the MyProject folder to a new name like **MyNewProjectName**
2. At the bottom of the root **CMakeLists.txt** file, add this: `add_subdirectory(MyNewProjectName)`
3. In your new directory, edit that **CMakeLists.txt**
   1. change your project name: `project(MyNewProject C)` 
   2. commment and uncomment your desired list of included SPL ".c" files (i.e. "stm8s_gpio.c")
4. In your header files, change "MyNewProject/inc/stm8s_conf.h"
   1. commment and uncomment your desired list of included SPL ".h" files (i.e. "stm8s_gpio.h")
5 Duplicate your build configuration from "MyProject OpenOCD" to "MyNewProject OpenOCD"
