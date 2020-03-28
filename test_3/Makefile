
##
## List here your source files (both .s, .c and .cpp)
##
SRC := main.cpp

##
## List here additional static libraries with relative path
##
LIBS :=

##
## List here additional include directories (in the form -Iinclude_dir)
##
INCLUDE_DIRS :=

##
## List here additional defines
##
DEFINES :=

##
## Define used to select target processor
##
TARGET :=

##
## System clock frequency, in hertz. Must be defined and set to correct value
## in order to make drivers working correctly
##
CLK_FREQ :=

##
## Optimization level
##
OPTLEVEL := -O0
#OPTLEVEL:= -O2
#OPTLEVEL:= -O3
#OPTLEVEL:= -Os

##
## Exceptions support. Uncomment to disable them and save code size
##
#EXCEPT := -fno-exceptions -fno-rtti -D__NO_EXCEPTIONS

##############################################################################
## You should not need to modify anything below                             ##
##############################################################################

LIB_SRC := ./startup.cpp ./libc_integration.cpp ../device/system_stm32f4xx.c

ALL_SRC := $(LIB_SRC) $(SRC)
CONFIGS := $(TARGET) $(CLK_FREQ) $(OPTLEVEL) -DDONT_USE_CMSIS_INIT

ifeq ("$(VERBOSE)","1")
Q := 
ECHO := @true
else
Q := @
ECHO := @echo
endif

## Replaces both "foo.cpp"-->"foo.o" and "foo.c"-->"foo.o"
OBJ := $(addsuffix .o, $(basename $(ALL_SRC)))

CXXFLAGS := $(INCLUDE_DIRS) -I../device -mcpu=cortex-m4 -mthumb $(CONFIGS) \
	    $(DEFINES) $(EXCEPT) -c -g -std=c++11
CFLAGS   := $(INCLUDE_DIRS) -I../device -mcpu=cortex-m4 -mthumb $(CONFIGS) \
	    $(DEFINES) $(EXCEPT) -c -g
AFLAGS   := -mcpu=cortex-m4 -mthumb
LFLAGS   := -mcpu=cortex-m4 -mthumb -Wl,--gc-sections -Wl,-Map,main.map \
 	    $(OPTLEVEL) -nostdlib -Wl,-T./linker_script.ld
DFLAGS   := -MMD -MP

LINK_LIBS := $(LIBS) -Wl,--start-group -lc -lgcc -Wl,--end-group

CC  := arm-none-eabi-gcc
CXX := arm-none-eabi-g++
AS  := arm-none-eabi-as
CP  := arm-none-eabi-objcopy
SZ  := arm-none-eabi-size

all: main.elf
	$(ECHO) "[CP  ] main.hex"
	$(Q)$(CP) -O ihex   main.elf main.hex
	$(ECHO) "[CP  ] main.bin"
	$(Q)$(CP) -O binary main.elf main.bin
	$(Q)$(SZ) main.elf

main.elf: $(OBJ) #all-recursive
	$(ECHO) "[LD  ] main.elf"
	$(Q)$(CXX) $(LFLAGS) -o main.elf $(OBJ) $(LINK_LIBS)

%.o: %.s
	$(ECHO) "[AS  ] $<"
	$(Q)$(AS)  $(AFLAGS) $< -o $@

%.o : %.c
	$(ECHO) "[CC  ] $<"
	$(Q)$(CC)  $(DFLAGS) $(CFLAGS) $< -o $@

%.o : %.cpp
	$(ECHO) "[CXX ] $<"
	$(Q)$(CXX) $(DFLAGS) $(CXXFLAGS) $< -o $@

clean:
	-rm -f $(OBJ) main.elf main.hex main.bin main.map $(OBJ:.o=.d)

#pull in dependecy info for existing .o files
-include $(OBJ:.o=.d)
