# MD380-boot-tests
Here we're trying to create custom firmware images for Tytera MD380, to be flashed and booted using stock recovery bootloader.  
Building tests requires arm-none-eabi-gcc.
  
  

### Test 1:
Simple baremetal image blinking simultaneously both the LEDs. Interrupts disabled, .data and .bss are empty.  
  
**Status**: BOOTS OK.
  
  
### Test 2:
Baremetal image with green LED fixed and red LED bliking.
Interrupts enabled, SCB_VTOR = 0x0800C000, .data and .bss initialised and libc integration.
  
**Status**: BOOTS OK.

  
### Test 3:
Interrupts enabled, SCB_VTOR = 0x0800C000, .data and .bss initialised, libc integration and call to CMSIS SystemInit() during startup.
Here we test interrupts: red LED is blinked in main(), while green LED is toggled inside SysTick interrupt handler.
*Note:* actually VTOR setting is hardcoded at line 182 of file "device/system_stm32f4xx.c", to be improved.
  
**Status**: TESTING.
