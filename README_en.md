--- File: Pasted ---
# Iris UNO-VG015
**Project available at [github](https://github.com/djObsidian/Iris-UNO-VG015)**  
**Project available at [gitflic](https://gitflic.ru/project/mikhvad/irisuno-vg015)**   
## Description
Iris UNO-VG015 is a minimal evaluation board featuring the domestic K1921VG015 microcontroller.  
![Top view](<images/IrisUNO_top_view.png>)  

## Habr Article
[Another evaluation board and K1921VG015 tests](https://habr.com/ru/articles/957832/)
## Youtube Video
[Iris UNO-VG015 evaluation board with a domestic microcontroller!](https://www.youtube.com/watch?v=FaNaOod3lDg)

## Features
- Compact form factor
- All microcontroller pins are broken out
- Arduino Uno rev3 connector support - compatibility with Arduino UNO and mbed shields
- Built-in programmer and CH347 USB-UART converter
- Built-in 4 MB* QSPI memory
- 4 LEDs connected to GPIO
- 16 MHz HSE crystal resonator
- 32.768 kHz LSE crystal resonator  
  
![Perspective view](<images/IrisUNO_perspective.png>)  
\*Manufacturer and capacity may vary, but not less than 4 MB. 

## Pin Assignments
### MCU Pinout

![MCU Pinout](<images/pinout.jpg>)  
Source - RP[1], pins are labeled on the board, GPIOs have a 'P' prefix (e.g., pin A2 is labeled on the board as PA2).
### External Connector Pinout

![alt text](images/pinout_wip_ext.png)
### Arduino UNO R3 Connector Pinout

![alt text](images/pinout_wip_int.png)

### Revision 0.4
Revision 0.4 contains bug fixes for revision 0.3. Main differences:
- Added a pull-up resistor on the D+ line for correct USB operation
- Fixed the USB-UART debugger connection; TRST and SRST are now separated and connected to the debugger
- Fixed the analog power supply; a ferrite filter has replaced the common-mode choke, and the ground is now common

### Revision 0.41  
Contains mainly PCB manufacturing improvements.
- Changed the external HSE crystal value to 16 MHz

## Schematic

[Schematic in PDF format, revision 0.3](IrisUNO.pdf)  
[Schematic in PDF format, revision 0.4](IrisUNO_0_4.pdf)

## Errata
Known issues are described in the `./errata.md` file.

## Quick Start

### 1. Installing the USB-UART Driver
Connect the board to your PC using a USB-C cable. Windows 10 and newer should automatically install the drivers for the USB-UART converter. If this does not happen, download and install the driver from the WCH website [3].  

### 2. Installing the USB-JTAG Driver
Install the WinUSB driver using Zadig [4] for the "USB to UART+JTAG (Interface 2)" device.  
![alt text](images/DriverInstall.png)  

### 3. Installing the IDE
Next, to install the Syntacore IDE development environment, follow steps 1-5 from Chapter 3 of the "Quick Start" guide [5].  

### 4. Installing OpenOCD
Install a custom OpenOCD build with K1921VG015 and CH347 support [6]. To do this, download the release version 20250928 (commit a01b00e) from the releases page, extract and overwrite the contents into the `tools` folder located inside the Syntacore IDE root folder `sc-dt`. This should be done after installing the `sc-dt_Patch_Niiet_Win32.zip` patch. Upon successful installation, the file `ch347t.cfg` should be present in the `\sc-dt\tools\share\openocd\scripts\interface` folder.  

### 5. Configuring the Debug Session
Follow step 7 from Chapter 3 of the Quick Start guide; use the following instead of the debugger configuration provided there:
 ```
-s ${eclipse_home}../tools/share/openocd/scripts
-s ${eclipse_home}../tools/share/openocd/scripts/interface/ftdi
-s ${eclipse_home}../tools/share/openocd/scripts/interface
-s ${eclipse_home}../tools/share/openocd/scripts/target
-f ch347t.cfg
-f k1921vg015.cfg
-c "init;halt"
 ```  
 
### 6. Project Settings for the Iris UNO Board  
LEDs are located on Port C. To run the `Run_leds` example (as well as some others), you must replace `GPIOA` with `GPIOC` in the appropriate places.  
*For board revisions 0.4 and 0.3:*  
To run NIIET RISC-V examples on the Iris UNO board, you need to change the symbols during the project build. Open the menu Project -> Properties -> C/C++ Build -> Settings -> GNU RISC-V C Compiler -> Preprocessor.  
![alt text](images/HSEclkval.png)  
Change the value of the `HSECLK_VAL` symbol to `12000000`.
To use `printf` with output redirected to UART, open the `retarget.c` file located in the project root and change the `SystemCoreClock_uart` define to the following:
```c
#define SystemCoreClock_uart	12000000
```
After this, the example is ready to run on the Iris UNO board.  

## Further Actions
You can refer to the NIIET manual for further information. When working with the VSCode extension, settings must be changed in a similar manner to use the CH347 debugger. For revisions 0.4 and 0.3, the crystal frequency must also be changed to 12 MHz.

## Resources and Sources
1. [Official product page on the manufacturer's website](https://niiet.ru/product/%D0%BA1921%D0%B2%D0%B3015/)
2. [Repository with SDK and documentation](https://gitflic.ru/project/niiet/niiet_riscv_sdk) 
3. [USB-UART driver download page for CH347](https://www.wch-ic.com/downloads/CH341SER_EXE.html)
4. [Official Zadig website](https://zadig.akeo.ie/)
5. [NIIET RISC-V Quick Start](https://niiet.ru/wp-content/uploads/2025/09/%D0%91%D1%8B%D1%81%D1%82%D1%80%D1%8B%D0%B9_%D1%81%D1%82%D0%B0%D1%80%D1%82_NIIET_RISCV-1.pdf)
6. [OpenOCD build by the RUS MCU community](https://gitflic.ru/project/rus_mcu/openocd/release)

[1]: https://niiet.ru/product/%D0%BA1921%D0%B2%D0%B3015/
[2]: https://gitflic.ru/project/niiet/niiet_riscv_sdk
[3]: https://www.wch-ic.com/downloads/CH341SER_EXE.html
[4]: https://zadig.akeo.ie/
[5]: https://niiet.ru/wp-content/uploads/2025/09/%D0%91%D1%8B%D1%81%D1%82%D1%80%D1%8B%D0%B9_%D1%81%D1%82%D0%B0%D1%80%D1%82_NIIET_RISCV-1.pdf
[6]: https://gitflic.ru/project/rus_mcu/openocd/release