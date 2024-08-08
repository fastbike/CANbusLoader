# CAN bus BootLoader

Bootloader to enable updating of applications on CAN bus nodes.

This has three parts:

1. Host application to enable connection to the CAN bus and check existing application loaded on a node and push out a new version to the node

2. Bootloader application to be installed on the node. This communicates with the Host application to enable query/upload of an MCU application

3. MCU Application Stub. This interacts with the Host and bootloader and has the ability to put the node into bootloader mode and rest so the Bootloader can execute the update. The application should build this functionality into the end user applications.

The use case initially is to target Renases RA4M1 micro controllers as this is the target architecture for the application the CAN bus is installing. However the general principles are universal, and the ideas build upon the initial code and protocol described in a [Maxim application note.]([How to Use the CAN Bootloader to Load User Application Code in the MAXQ7665A | Analog Devices](https://www.analog.com/en/resources/app-notes/how-to-use-the-can-bootloader-to-load-user-application-code-in-the-maxq7665a.html))

There are a number of useful CAN bootloader repos on Github targetting ARM micros



The following Requirements must be met:

- Bootloader to load on power up and decide if application should be run or bootloader

- Bootloader to only switch to application mode if application in Flash matches the hash/signature previously uploaded, otherwise to propogate an error message on a regular basis

- Bootloader not able to be overwritten which would leave the node in an irrecoverable state

- Bootloader to be able to receive application code and write to Flash

- Bootloader to be able to compute a hash/signature on uploaded code and update local copy if verified

- Bootloader to send ACK/NACK with optional status/error data after each command

- MCU application able to receive a specific CAN message to switch into bootloader mode, and reset the mcu
