# Tof and TDoA for dwm1001

This Firmware allows the Decawave DWM1001-Dev devices to obtain the TOF and TDOA information needed for localization. RTOS is disabled in this FW to help localization faster than 10Hz, the distance response from Anchor will be automatically sent as serial under baudrate 115200  with frequency 100Hz. For the localization by UWB please refer: https://github.com/vuthanhcdt/localization-uwb-ros

## Overview
The project is built based on a example from Decawave as follow : 
```
custom-dwm1001-uwb-firmwares/
├── boards            // DWM1001-DEV board specific definitions
├── deca_driver       // DW1000 API software package 2.04 
├── examples          // C simple examples 
│   ├── ss_twr_init   // Single Sided Two Way Ranging Initiator for Anchor
│   ├── ss_twr_resp   // Single Sided Two Way Ranging Responder for Tag
├── nRF5_SDK_14.2.0   // Nordic Semiconductor SDK 14.2 for nrF52832
└── README.md
```

## How to program the devices
Follow these steps to load the programe to Anchor and Tag on the DWM1001-DEV boards.

### Tag
1. Open the project inside `custom-dwm1001-uwb-firmware/examples/ss_twr_init/SES`by Segger
2. Rebuild and download

### Anchor
1. Open the project inside `custom-dwm1001-uwb-firmware/examples/ss_twr_init/SES`by Segger
2. Click ss_init_main.c, go to line 155 to change an ID of Anchor, the ID should be A1, A2...
3. Rebuild and download
4. Using a serial application to display the distance to Tag, if the distance get accuracy more than 10 cm, do step 5
5. Click main.c, go to line 60 to calibrate antten delay (TX_ANT_DLY, RX_ANT_DLY)
6. Rebuild and download

## Supported IDE

**NOTE:** We recomment using SES 5.64
The examples are ready to use with the following IDE :
* Segger Embedded Studio (SES)
* Keil KEIL µVision

## Segger Embedded Studio

Each example contains a emproject project file for SES. The examples compile and load cleanly to the DWM1001.   
The project was created with the SES version V3.34a. 

SES has a free license for nrF52832 development. Consequently, this IDE can be used without any limitation for DWM1001 development.

For more information regarding Segger Embedded Studio, please visit https://www.segger.com/products/development-tools/embedded-studio/

For more information about free license for nrF52832, please read https://www.nordicsemi.com/News/News-releases/Product-Related-News/Nordic-Semiconductor-adds-Embedded-Studio-IDE-support-for-nRF51-and-nRF52-SoC-development

### SES : Additional Package

When using SES IDE, you will need to install the following package :

Package                                                                                                                           
CMSIS 5 CMSIS-CORE Support Package (version 5.02)                                                                           
CMSIS-CORE Support Package (version 4.05)                                                                           
Nordic Semiconductor nRF CPU Support Package (version 1.06)                                                                           

They can be install from SES itself, through the package manager in the tools menu. 

## Restore the factory firmware

In order to store the factory firmware, first download the `.hex` file from

```
https://www.qorvo.com/products/d/da008479
```

Extract the ZIP. You will find the `.hex` factory firmware file in `DWM1001/Factory_Firmware_Image/DWM1001_PANS_R2.0.hex`.

Open SEGGER J-Flash Lite (tested with V6.62a, the executable file is `JFlashLiteExe`) and choose the following fields:

1. Device: `NRF52832_XXAA`
2. Interface: `SWD`
3. Speed: `4000 kHz`
4. Data File: `DWM1001_PANS_R2.0.hex` (downloaded following steps above)

Connect the DWM1001-DEV board to a USB port (we found some issues if you connect it after you start JFlashLite, so we recommend that you connect the dev board **BEFORE** starting JFlashLite). Optionally, click first on `Erase Chip`.

Click on `Program Device`. If you also erased the previous board, you should see the following output:

```
Connecting to J-Link...Connecting to target...Erasing...Done
Conecting to J-Link...Connecting to target...Downloading...Done
```

**NOTE:** The LEDs on the DEV board will be OFF while the firmware is being updated, **do not disconnect the board** until you see the `Done` message and the LEDs are flashing again. After erasing the device, you should see a single non-blinking red LED on the DEV board.
