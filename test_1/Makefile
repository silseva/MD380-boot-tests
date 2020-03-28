 
CC  := arm-none-eabi-gcc
CXX := arm-none-eabi-g++
AS  := arm-none-eabi-as
CP  := arm-none-eabi-objcopy
SZ  := arm-none-eabi-size

CPU := -mcpu=cortex-m4 -mthumb -mfloat-abi=hard -mfpu=fpv4-sp-d16

AFLAGS   := $(CPU)
CFLAGS   := $(CPU) -O0 -g -c -I../device
CXXFLAGS := $(CPU) -O0 -fno-exceptions -fno-rtti -g -c -I../device
LFLAGS   := $(CPU) -Wl,-T./linker.ld,-Map,main.map -nostdlib

SRC := main.cpp
OBJ := $(addsuffix .o, $(basename $(SRC)))

all: $(OBJ)
	$(CXX) $(LFLAGS) -o main.elf $(OBJ)
	$(CP) -O binary main.elf main.bin
	$(SZ) main.elf

clean:
	-rm $(OBJ) main.elf main.bin main.map

%.o : %.s
	$(AS) $(AFLAGS) $< -o $@

%.o : %.c
	$(CC) $(CFLAGS) $< -o $@

%.o : %.cpp
	$(CXX) $(CXXFLAGS) $< -o $@
