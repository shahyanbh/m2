#******************************************************************************
# Copyright (C) 2017 by Alex Fosdick - University of Colorado
#
# Redistribution, modification or use of this software in source or binary
# forms is permitted as long as the files maintain this copyright. Users are 
# permitted to modify this and use it to learn about the field of embedded
# software. Alex Fosdick and the University of Colorado are not liable for any
# misuse of this material. 
#
#*****************************************************************************

#------------------------------------------------------------------------------
# <Put a Description Here>
#
# Use: make [TARGET] [PLATFORM-OVERRIDES]
#
# Build Targets:
#      <FILE>.o - Builds <FILE>.o object file
#      <FILE>.asm - builds <FILE>.asm assembly file
#      <FILE>.i - builds <FILE>.i preprocessor file
#
# Platform Overrides:
#      PLATFORM - for cross compilation (MSP432, HOST)
#
#------------------------------------------------------------------------------
include sources.mk

PLATFORM = MSP432

# Architectures Specific Flags
TARGET = c1m2
GFLAGS= -Wall -O0 -g -std=c99 
CPPFLAGS = -D$(PLATFORM) $(INCLUDES)

ifeq ($(PLATFORM),MSP432)
        CC = arm-none-eabi-gcc 
        LD = arm-none-eabi-ld
        LDFLAGS = -Wl,-Map=$(TARGET).map -L ../ $(LINKER_FILE) 
        CFLAGS = -mcpu=$(CPU) -m$(ARCH) --specs=nosys.specs 
        LINKER_FILE = -T msp432p401r.lds
        CPU = cortex-m0plus
        ARCH = thumb
        SPECS = nosys.spec
        SIZE = arm-none-eabi-size
else
        CC = gcc
        LDFLAGS = -Wl,-Map=$(TARGET).map	
        CFLAGS = $(GFLAGS)

endif

OBJS = $(SOURCES:.c=.o)

PP = $(SOURCES:.c=.i)

ASM = $(SOURCES:.c=.asm)


%.o : %.c
	$(CC) -c $< $(CPPFLAGS) $(CLFAGS) $(GFLAGS) -o $@

%.i : %.c
	$(CC) -E $< $(CPPFLAGS) $(CLFAGS) $(GFLAGS) -o $@

%.asm : %.c
	$(CC) -S $< $(CPPFLAGS) $(CLFAGS) $(GFLAGS) -o $@


.PHONY: compile-all
compile-all: build

.PHONY: build
all: $(TARGET).out

$(TARGET).out: $(OBJS)
	$(CC) $(OBJS) $(CFLAGS) $(CPPFLAGS) $(LDFLAGS) -o $@

.PHONY: clean
clean:
	rm -f $(OBJS) $(TARGET).out $(TARGET).map
.PHONY: size
size:	
	$(SIZE) -Atd $(TARGET).out
       
