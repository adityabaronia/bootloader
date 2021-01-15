# Bootloader
Bootloader is a small application on flash that helps use to flash the main program in flash memory. yup that means on a flash memory the will be 2 images:
1. Bootloader
2. Firemware/Application

Since the bootloader and the main firmware both are on same flash memory so the prograamer should keep in mind that bootloader's code should be precise and small.
## Typical sequence for flashing a controller
1. open image flashing tool
2. Start the bootloader
3. erase the flash
4. Send the binary file information to bootloader
5. Generate checksum
6. exit the bootloader and enter into application
## Working of Bootloader
A basic bootloader should:
1. communicate to peripherals(I2C, SPI, USB, UART, CAN)
2. read/write flash memory and EEPROM

### Branching code
At the start of microcontroller there is possibility that there are more 2 images loaded in memory, the bootloader and the application.
So, it is necessary that as a part of bootloader there is a braching algorithm that can handle which image is to be load/executed.

There are different methods that can be used to decide which image to load:
1. **Use the GPIO lines to make the decision**. Example:- If the GPIO is high, load the application; if GPIO is low, load the bootloader.
      The logic signal is checked. If the signal is low, top of the stack is loaded and the program flow is transfered to bootloader main function. If the logic signal is high, the application reset vector is loadedand then executed.
2.  

branching code is responsible for execution of bootloader