# MD380-boot-tests
Here we're trying to create custom firmware images for Tytera MD380, to be flashed and booted using stock recovery bootloader.  
Building tests requires arm-none-eabi-gcc.
  
  

### Test 1:
Simple baremetal image blinking simultaneously both the LEDs. Interrupts disabled, .data and .bss are empty.  
  
**Status**: BOOTS OK.
  
  
### Test 2:
Baremetal image with green LED fixed and red LED bliking.
Interrupts enabled, SCB_VTOR = 0x0800C000, .data and .bss initialised and libc integration.
  
**Status**: TESTING.
