# Bootloader

Bootloader is a small application on flash that helps use to flash the main program in flash memory. yup that means on a flash memory the will be 2 images:
1. Bootloader
2. Firemware/Application

Since the bootloader and the main firmware both are on same flash memory so the prograamer should keep in mind that bootloader's code should be precise and small.

## Working of Bootloader
A basic bootloader should:
1. communicate to peripherals(I2C, SPI, USB, UART, CAN)
2. read/write flash memory and EEPROM

### Branching code