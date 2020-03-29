#/*
# * FreeRTOS Kernel V10.3.1
# * Copyright (C) 2020 Amazon.com, Inc. or its affiliates.  All Rights Reserved.
# *
# * Permission is hereby granted, free of charge, to any person obtaining a copy of
# * this software and associated documentation files (the "Software"), to deal in
# * the Software without restriction, including without limitation the rights to
# * use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of
# * the Software, and to permit persons to whom the Software is furnished to do so,
# * subject to the following conditions:
# *
# * The above copyright notice and this permission notice shall be included in all
# * copies or substantial portions of the Software.
# *
# * THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
# * IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS
# * FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR
# * COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER
# * IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN
# * CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.
# *
# * http://www.FreeRTOS.org
# * http://aws.amazon.com/freertos
# *
# * 1 tab == 4 spaces!
# */

CC=arm-none-eabi-gcc
OBJCOPY=arm-none-eabi-objcopy
ARCH=arm-none-eabi-ar
WARNINGS=-Wall -Wextra -Wshadow -Wpointer-arith -Wbad-function-cast -Wcast-align -Wsign-compare \
		-Waggregate-return -Wstrict-prototypes -Wmissing-prototypes -Wmissing-declarations -Wunused

ifeq ("$(VERBOSE)","1")
Q := 
ECHO := @true
else
Q := @
ECHO := @echo
endif

#
# Includes dirs
#
INCLUDES= -I. -I../../Source/include \
		  -I../../Source/portable/GCC/ARM_CM4F \
		  -I../../Demo/CORTEX_M4F_STM32F407ZG-SK/board \
		  -I../Common/include \
		  -I./Libraries/CMSIS/Device/ST/STM32F4xx/Include \
		  -I./Libraries/STM32F4xx_StdPeriph_Driver/inc

##
## Optimization level
##
OPTLEVEL := -O0
#OPTLEVEL:= -O2
#OPTLEVEL:= -O3
#OPTLEVEL:= -Os

#
# Compiler flags
#
CFLAGS=$(OPTLEVEL) $(WARNINGS) $(INCLUDES) $(DEBUG) \
	   -D__weak='__attribute__((weak))' \
	   -mcpu=cortex-m4 -mthumb -mfloat-abi=hard -mfpu=fpv4-sp-d16 \
	   -fomit-frame-pointer -fno-strict-aliasing -fno-dwarf2-cfi-asm \
	   --specs=nosys.specs

AFLAGS   := -mcpu=cortex-m4 -mthumb

LINKER_FLAGS=-Xlinker -ortosdemo.elf -Xlinker -M -Xlinker -Map=rtosdemo.map

RTOS_SOURCE_DIR=../../Source
DEMO_SOURCE_DIR=../Common/Minimal
#
# Source files
#
SRC = \
main.c \
ParTest.c \
stm32f4xx_it.c \
$(DEMO_SOURCE_DIR)/integer.c \
$(DEMO_SOURCE_DIR)/flash.c \
$(DEMO_SOURCE_DIR)/PollQ.c \
$(DEMO_SOURCE_DIR)/comtest.c \
$(DEMO_SOURCE_DIR)/flop.c \
$(DEMO_SOURCE_DIR)/semtest.c \
$(DEMO_SOURCE_DIR)/dynamic.c \
$(DEMO_SOURCE_DIR)/BlockQ.c \
$(DEMO_SOURCE_DIR)/GenQTest.c \
$(DEMO_SOURCE_DIR)/blocktim.c \
$(DEMO_SOURCE_DIR)/recmutex.c \
$(DEMO_SOURCE_DIR)/death.c \
$(RTOS_SOURCE_DIR)/tasks.c \
$(RTOS_SOURCE_DIR)/queue.c \
$(RTOS_SOURCE_DIR)/list.c \
$(RTOS_SOURCE_DIR)/timers.c \
$(RTOS_SOURCE_DIR)/portable/MemMang/heap_2.c \
$(RTOS_SOURCE_DIR)/portable/GCC/ARM_CM4F/port.c \
startup/system_stm32f4xx.c \
Libraries/STM32F4xx_StdPeriph_Driver/src/misc.c \
board/iar_stm32f407zg_sk.c

#
# Define all object files.
#
OBJ = $(SRC:.c=.o)

%.o: %.s
	$(ECHO) "[AS  ] $<"
	$(Q)$(AS)  $(AFLAGS) $< -o $@

%.o : %.c
	$(ECHO) "[CC  ] $<"
	$(Q)$(CC)  $(CFLAGS) $< -o $@

all: rtosdemo.elf
	$(ECHO) "[CP  ] rtosdemo.hex"
	$(Q)$(CP) -O ihex   rtosdemo.elf rtosdemo.hex
	$(ECHO) "[CP  ] rtosdemo.bin"
	$(Q)$(CP) -O binary rtosdemo.elf rtosdemo.bin
	$(Q)$(SZ) rtosdemo.elf

rtosdemo.hex : rtosdemo.elf
	$(OBJCOPY) rtosdemo.elf -O ihex rtosdemo.hex

rtosdemo.elf : $(OBJ) Makefile
	$(CC) $(CFLAGS) -o rtosdemo.elf $(OBJ) $(LINKER_FLAGS)

clean :
	rm -rf $(OBJ) $(OBJ:.o=.d)
	touch Makefile
