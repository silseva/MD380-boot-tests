# MD380-boot-tests
Here we're trying to create custom firmware images for Tytera MD380, to be flashed and booted using stock recovery bootloader.  
Building tests requires arm-none-eabi-gcc.

### Test 1:
Simple baremetal image blinking simultaneously both the LEDs. Interrupts disabled, .data and .bss are empty.  
**Status**: BOOTS OK.
