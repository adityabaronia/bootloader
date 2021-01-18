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
      The logic signal is checked. If the signal is low, top of the stack is loaded and the program flow is transfered to bootloader main function. If the logic signal is high, the application reset vector is loaded and then executed.

*Question: What happens if the bootloader is loaed in memory but the application hasn't and there is request to jump to the application?*
Ans: To avoid this kind of situation bootloader should check whether the application rest vector exists or not.

2. The above question brigns up most important braching check. which is, whether the application vector exists or not. By this extra check on the application vector the programmer can save the micro-controller to jump on a non existing application.
3. Instead of checking the GPIO programmer can program to check the EEPROM. If a particular value is set in a EEPROM it means  execution has to jump to boot-loader. Example: Application code is running and in between there is software reset. Before the rest is done the application code can change some value in EEPROM. Let that value be 'B'. This means branching code should jump the exectution to bootloader. When execution jumps to branching code it checks for application reset vector and the value in EEPROM accordingly it take the decision.

This branching code can be integrated with bootloader then it can work more robustly.
**What all a branching code can check:**
1. Does application reset vector exists?
2. Is the mode set to 'B'?
3. Is a programing tool present? For this branching code can send out message on peripheral and check for the acknowledgement. 