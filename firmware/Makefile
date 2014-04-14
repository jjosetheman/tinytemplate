# Makefile for the TinyGB firmware
#----------------------------------------------------------------------------
# 22-Dec-2013 ShaneG
#
# You will need avr-gcc for this.
#----------------------------------------------------------------------------
BASE_DIR := $(dir $(lastword $(MAKEFILE_LIST)))

# CPU specific options
MCU       = attiny85
F_CPU     = 8000000

# Define tools and options
# All you really need to do is change the TARGET name
TARGET  := attiny85
SIZE    := avr-size
CXX     := avr-gcc
CFLAGS  := -std=gnu99 -Wall -Os -mmcu=$(MCU) -DF_CPU=$(F_CPU) -I$(BASE_DIR)/include
LDFLAGS := -mmcu=$(MCU)
OBJCOPY := avr-objcopy
MBFLASH := tools/mbflash.py

# Main source
SOURCES = \
  main.c

# Shared source
SHARED = \
  shared/uart_tiny.c

# Derive object file names
OBJECTS  = $(filter-out $(SOURCES), $(patsubst %.c,%.o,$(SOURCES)) $(patsubst %.S,%.o,$(SOURCES)))
OBJECTS += $(filter-out $(SHARED), $(patsubst %.c,%.o,$(SHARED)) $(patsubst %.S,%.o,$(SHARED)))

.PHONY: all clean

all: $(TARGET).hex

clean:
	@rm -f $(OBJECTS) $(TARGET).hex $(TARGET).elf

flash: $(TARGET).hex
	@$(MBFLASH) -d $(MCU) $(TARGET).hex


$(TARGET).elf: $(OBJECTS)
	@echo Linking
	@$(CXX) $(LDFLAGS) -o $@ $(OBJECTS)
	@$(SIZE) --mcu=$(MCU) --format=avr $@

$(TARGET).hex: $(TARGET).elf
	@echo Creating Hex File
	@$(OBJCOPY) -j .text -j .data -O ihex $< $@

# How to compile
%.o: %.c
	@echo Compiling $<
	@$(CXX) $(CFLAGS) -c $< -o $@

%.o: %.S
	@echo Assembling $<
	@$(CXX) $(CFLAGS) -c $< -o $@