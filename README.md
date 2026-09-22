![Logo](/readme_images/Flag.png)
### Canadian-designed open source hardware. For everyone.

# FlexiHAL 2350 CNC Controller
![Logo](/readme_images/logo_sm.jpg)
<img src="/readme_images/Board_Photo.jpg" width="800">


Expatria Technologies GRBLHAL and LinuxCNC (and more!) CNC control board

Currently available in our online store:

[https://expatria.myshopify.com/products/flexi-hal-2350](https://expatria.myshopify.com/products/flexihal-2350-cnc-controller-rev-a3-1-for-grblhal-and-linuxcnc)

Please consider buying a board to support our open-source designs. 

The FlexiHal 2350 is designed to be an EMI resistant IO platform for any microcontroller based CNC/motion control firmware or software.  This board includes a few features that we couldn't find on other boards, and it reduces the amount of extra wiring in our setups.  In the co-operative spirit of the PrintNC and other CNC communities, and Open Source Hardware, the FlexiHAL will be licensed and free to use by all parties, including commercial parties, under the CERN-OHL-S V2 license.  It is our hope that the community finds the design useful and that it may be carried forward to help advance the PrintNC and broader CNC hobby community.

The FlexiHAL 2350 is a premium controller that utilizes Raspberry Pi's RP2350 microcontroller.  The RP2350 is well suited for CNC applications using step/direction or quadrature outputs when leveraging the unique capabilities of the PIO cores.    In addition, the Flexi-HAL is intended to be used with the Expatria Real-Time jog controller or similar peripheral:



https://github.com/Expatria-Technologies/RT_Jog_Controller

The key features of the FlexiHAL 2350:

1) 6 Axis of isolated step/dir motor control featuring high-speed digital isolators and differential signal drivers for maximum signal integrity and step rates.
2) 15 general inputs (user+limits+ 2 encoders)
3) 40 points of I/O not including step/dir
4) Integrated support for 3 wire inductive type powered switches.
5) Onboard 5V power regulator.
6) Integrated RS485 with automatic direction control.
7) Support for closed loop stepper motors and servos with alarm feedback.   6 discrete inputs for motor alarm.  6 discrete outputs for motor enable.
8) Two Differential interfaces for a spindle encoder inputs or can be used for general purpose inputs.
9) Real-time control port for remote pendants.
10) Raspberry Pi GPIO connector allows integration of sender software and allows the board to host a full LinuxCNC installation.
11) 5V PWM laser control.  4 additional high-speed 5V outputs.
12) Flood/Mist/Spindle relay drivers.
13) Additional auxilliary relay driver outputs.  8 high-current outputs total.
14) All machine facing IO is galvanically isolated from the MCU and user interfaces.
15) Easy reliable USB-C connection to a PC
16) GRBLHAL Ethernet Websockets or Telnet communication through onboard Ethernet.
17) GRBLHAL SD card G-Code streaming and macro/subroutine storage (including looping and conditional execution) with onboard storage and SD card.

Prebuilt Raspberry Pi LinuxCNC image is located here:  
https://github.com/Expatria-Technologies/Flexi-Pi/releases

Accessory Encoder Breakout PCB is located here:  
[https://github.com/Expatria-Technologies/EST_Accessory_PCB/ENCODER_BREAKOUT_MK2](https://github.com/Expatria-Technologies/EST_Accessory_PCB/tree/main/ENCODER_BREAKOUT_MK2)

Various community mods and accessories are located here:  
https://github.com/Expatria-Technologies/Mods-Accessories

Recommended GRBLHAL Post Processor is here:  
https://github.com/Dietz0r/grblHAL_Fusion360_Post_Processor

## Flexi-HAL Overview

<img src="/readme_images/Board_Overview.png" width="700">

### Pinout List:
This diagram shows which pins connect directly to the RP2350 and which are connected to the RP2040 FlexiGPIO expander.
<img src="/readme_images/Pinout.jpg" width="900">

### RP2350 Microcontroller and RP2040 FlexGPIO expander.
<img src="/readme_images/rp2350.webp" width="300">

The FlexiHAL2350 is built around the RP2350 from Raspberry Pi, an MCU we're genuinely excited about, for its versatile PIO cores and the unique capabilities they unlock.
The board is designed with flexibility at its core, supporting both GRBLHAL and LinuxCNC. In the future we would also like to add support for uCNC.  Pre-built binary firmware for a variety of axis configurations will be made available on the Expatria GitHub, so you can get up and running quickly without compiling from scratch.
For LinuxCNC users, there is a customized port of the Remora project alongside FlexiHAL, making it straightforward to switch between motion control environments. In this mode a Raspberry Pi 5 is recommended when running the Expatria Flexi-Pi image.

In order to expand the amount of I/O available on the board, a custom I2C GPIO expander is implemented on an additional RP2040 MCU.  This allows for a large number of additional IO points with much greater potential flexibility than is offered by traditional GPIO expanders.  It will be possible to implment functions such as additional analog/PWM outputs, nexopixel outputs and additional stepper outputs by extending the FlexiGPIO code.

FlexGPIO source code is posted here:  
[https://github.com/Expatria-Technologies/FlexGPIO](https://github.com/Expatria-Technologies/FlexGPIO)

FlexGPIO GRBLHAL plugin is posted here:  
[https://github.com/Expatria-Technologies/plugin_FlexGPIO](https://github.com/Expatria-Technologies/plugin_FlexGPIO)

### UF2 Bootloader
The FlexiHAL 2350 uses the RP2350's built-in UF2 bootloader. This allows you to upgrade or change the firmware on the flexi as easily as copying a file to a USB drive.  Pre-built binary firmwares from Expatria are distributed as UF2 files.  To enter UF2 mode hold the BOOT button while powering up the board or while pulsing the RUN button.  Note that there are bootloader buttons for both the RP2040 based FlexGPIO expander as well as the main RP2350 processor.

Once in bootloader mode, the Flexi will appear as a USB storage device called "RPI-RP2."  Simply copy the new firwmare to this USB drive and the board will automatically install it and reboot.  Some operating systems may give an error when flashing is complete because the 'disk' was removed without ejecting it.  This error can be ignored.

### Power Input
<img src="/readme_images/isolation_zones.png" width="600">

The Flexi-HAL features the capability for full power and ground isolation between the sensitive microncontroller and host circuits, and the external IO that extends out to the rest of the machine.  There is a single input for 12-24VDC.  The board has its own onboard 5V regulator to power the stepper drivers and external RS485 interface.  There is also a small capacity 12V LDO that is specifically for driving the limit and user switches.

Flexi-HAL has reverse polarity as well as over-current protection beyond 1.75A.  This is important to consider when using external relays that draw a lot of current as this may overwhelm the capacity of the board.  If you need to drive more than 250 mA through the auxillary and mist/coolant relay outputs, external relays are likely required.

<img src="/readme_images/power_bypass.jpg" width="500">
Normally the MCU and RPI header will be powered via the USBC connector.  The RT control interface is also powered from the isolated domain.  By installing two jumpers on the above offset pins, the 5V power and ground isolation can be bypassed and the Flexi-HAL will operate without an external 5V supply in a semi-isolated state.  This does reduce the EMI resistance of the board and is not recommended when sending Gcode via the USBC connector.

Note: With the isolation jumpers not-populatd, and the Flexi-HAL connected to 12-24v power, it may appear that the board is ready to run as some LEDs illuminate. You MUST provide 5v power to the isolated domain (MCU, Jogger) either through USB or the bypass jumpers in order for the MCU and Jogger to turn on.

### Stepper Drivers
<img src="/readme_images/Stepper_Pins.jpg" width="400">

The stepper drivers are designed to be used with standard or flex rated RJ45 cables.  Unfortunately you will need to ensure that at the external driver the high and low signal pairs are connected correctly as there is no standard pinout on these drivers.  The 8 pin connection allows you to run a high and low pair for every signal to ensure the best possible signal integrity.  The Flexi-HAL uses high speed digital isolators and differential RS-422 style signal drivers for the motion signals.

Dedicated enable and alarm inputs are routed through the FlexGPIO expander.  A specific motor alarm interrupt is provided to immediately notify the host processor.  Alarm and enable can be set to any polarity that is required for operation.  There are LEDs provided on the board for every motor signal to help with debugging wiring issues.

Typical wiring for most open-loop stepper drivers: 

![image](https://github.com/Expatria-Technologies/Flexi-HAL/assets/6061539/89e5df1e-06ef-4319-acb0-a30ee8e6447b)

### Auxillary Outputs
<img src="/readme_images/highpower-lowpower.jpg" width="400">
Two style of outputs are provided: high power and low power.  High power outputs are driven directly from the board input power (12-24V).  Low power outputs are driven at 5V.  All outputs are PNP style high-side switching.

#### High Power Outputs
Any single high-power output can drive up to 700 mA, though the total current supplied by the board is a maximum of 1.75A.  Each automotive grade output has short circuit, over-current and thermal shutdown protection.

#### Low Power Outputs
The low power outputs are designed for fast switching applications such as PWM output, neopixels or even additional stepgens.  They should not be configured to drive more than 10 mA.  They are provided with limited over-voltage and short-circuit protection.

### PWM Spindle Control

<img src="/readme_images/Spindle_PWM_Config.jpg" width="400">

The spindle PWM pin is directly connected to the RP2350 main MCU.  This is normally used to drive a laser module with pulse modulation.  To use this output with a 0-10V analog spindle control, an external PWM converter must be used.

### RS485 Spindle Control
This interface is primarily intended to be used with a Huanyang style VFD for spindle control.  The A, B and G (common) pins are marked on the top and bottoms sides of the PCB.  Simply connect the appropriate pins to the terminals on the VFD.  Note that the G pin should be used for signal common, it should not be connected to the shield of a shielded cable.

https://github.com/grblHAL/Plugins_spindle/

### 6 Axis limit and auxillary inputs

A total of 9 direct 3 wire opto-coupled inputs are provided to cover limit and auxillary inputs.  Both PNP and NPN switches are supported.  A selector switch is provided on each input.

Auto-squaring is supported by enabling ganged axes in GRBLHAL and setting the appropriate pins.

In addition two probe inputs are provided that are connected to the FlexGPIO with a dedicated interrupt signal.  Polarity and enable masks are fully configurable.

### HALT Input signal and Polarity Selection

The HALT signal is not a safety feature and should not be used in place of a true electrical emergerncy stop.  It is intended to notify the controller of urgent requests.  The halt signal is shared between the PCB terminal and the RT control interface, but the polarity and operation is freely configurable between NPN, PNP, NO and NC.

<img src="/readme_images/haltsel.png" width="400">

If you are in doubt of the correct header position for your case, you may simply try both positions and choose the one where the HALT signal is not asserted (red light is not on) when in the nominal operating condition.

### Real-Time Control Port
<img src="/readme_images/Jog2k_Enclosure_2.png" width="500">
This port is intended to allow for external pendant type devices to issue real-time jogging and override controls to the motion  controller.  It uses I2C signalling and adds additional signals for the keypad interrupt as well as the Halt signal.  We feel that a robust and wired control is the safest way to interact with a CNC machine in real time.  A simple reference controller implementation is under active development, but there are some additional code examples referenced in the GRBLHAL I2C keypad plugin repository:

https://github.com/grblHAL/Plugin_I2C_keypad/

https://github.com/Expatria-Technologies/RT_Jog_Controller/

### Encoder Input Ports
The two encoder ports are used in either of two ways.  First, these ports allow a differential connection to an external encoder for a robust GRBLHAL lathe implementation or to support a high-speed encoder input for LinuxCNC.  An encoder such as E6B2-CWZ1X is suitable for most spindle applications.  Additionally, an encoder based isolated ADC module can be used for Plasma THC applications.

The RJ45 pinout:  
<img src="/readme_images/encoder_rj45_pinout.jpg" width="250">

There is also a small breakout available that exposes these signals for use as discrete opto-coupled inputs.  This can be useful when you have a group of signals that are remotely co-located (such as the sensors in an ATC spindle) so that you only need to bring one cable back to the mainboard.

Encoder Breakout Board:  
[https://github.com/Expatria-Technologies/EST_Accessory_PCB/tree/main/ENCODER_BREAKOUT_MK2](https://github.com/Expatria-Technologies/EST_Accessory_PCB/tree/main/ENCODER_BREAKOUT_MK2)
<img src="/readme_images/encoder_render.jpg" width="400">  


### FlexIO Port
The FlexiHAL2350 introduces a new type of expansion port for Expatria.  The FlexIO port multiplexes the RS485 interface independently to a second connector that carries both power and RS485 signals.  This can be used as both a power source for external modules that are powered from the FlexiHAL's 24V input, or it can serve as a power supply for the board.  The RS485 signalling can be used with Modbus server devices such as the PicoHAL running the nanoModbus GRBLHAL plugin to provide independent control of toolchanges, LEDs, fans etc.

The RJ45 pinout:  
<img src="/readme_images/FlexIO_Pinout.jpg" width="150">

Details on the PicoHAL are posted here:  
[https://github.com/Expatria-Technologies/PicoHAL_HW](https://github.com/Expatria-Technologies/PicoHAL_HW)

NanoModbus GRBLHAL Plugin is posted here:  
[https://github.com/Expatria-Technologies/grblhal_nmbs_plugin](https://github.com/Expatria-Technologies/grblhal_nmbs_plugin)

nanoModbus toolchanger in action; also showing Sienci's ATCI plugin.  In this video, the LEDs and all motions of the tool carousel are controlled via RS485 Modbus.



https://github.com/user-attachments/assets/e5c6b0d1-849d-4eac-88a9-3914cdb023e6



### Raspberry PI expansion header
The Rasberry Pi GPIO header allows the Flexi-HAL to host a full Raspberry Pi type SBC.  This allows the platform to support LinuxCNC via the Remora project.  Power should be connected to either the Pi or to the FlexiHAL, not both.

<img src="/readme_images/Pi_Pinout.jpg" width="500">

### Accessories
Some 3D printed accessories are avilable in [Mods & Accessories](https://github.com/Expatria-Technologies/Mods-Accessories/), including a DIN rail mount and enclosures/mounts for the limit and button breakouts.

### Default Jumper Locations
There are no jumpers needed for the operation of this board.  The only jumpers are for the 5V power bypass.

### Example Wiring Diagram
A comprehensive wiring diagram has been developed by the PrintNC community using the FlexiHAL.  While not specific to Flexi, this gives a great example of how to connect the board into the rest of a complete electronics box to drive a CNC machine. 
https://wiki.printnc.info/en/v3/wiring

### Attributions
This project uses components from the very helpful actiBMS library.

https://github.com/actiBMS/JLCSMT_LIB



