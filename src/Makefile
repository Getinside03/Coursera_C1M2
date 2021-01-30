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
# Makefile that can natively and cross compile an application. This file can generate
# preprocessor/assembly/object/dependency/executable and map output files.
#
# Use: make [TARGET] [PLATFORM-OVERRIDES]
#
# Build Targets:
#     <FILE>.o - Builds <FILE>.o object file
#	  <FILE>.i - Builds <FILE>.i preprocessed file
#	  <FILE>.asm - Generate assembly output of c-program implementation files and the final output executable.
#	  build - Compile all object files and link into a final executable.
#	  compile-all - Compile all object files, but do not link.
#	  clean - Remove all compiled objects, preprocessed outputs, assembly outputs, executable files and build output files.
#
# Platform Overrides:
#      CPU - ARM Cortex Architecture (cortex-m4)
#	   ARCH - ARM Architecture (arm)
#	   SPECS - Specs file to give the linkes(nosys.specs)
#
#------------------------------------------------------------------------------
include sources.mk

OBJS=	$(SOURCES:.c=.o)
TARGET= c1m2.out

# Case PLATFORM=MSP432
ifeq	($(PLATFORM),MSP432)
	# Architectures Specific Flags
	LINKER_FILE=-T msp432p401r.lds
	CPU=cortex-m4
	ARCH=arm
	SPECS=nosys.specs

	# Compiler Flags and Defines
	CC=arm-none-eabi-gcc
	LD=arm-none-eabi-ld
	CFLAGS =-DMSP432
endif

# Case PLATFORM=HOST
ifeq	($(PLATFORM),HOST)
	# Architectures Specific Flags
	SPECS=nosys.specs

	# Compiler Flags and Defines
	CC=gcc
	LD=ld
	CFLAGS =-DHOST
endif

%.i: %.c
	$(CC) -E $^ -o $@ $(CFLAGS) $(INCLUDES)

%.asm: %.c
	$(CC) -S $^  $(CFLAGS) $(INCLUDES)

%.o: %.c
	$(CC) -c $^ -o $@ $(CFLAGS) $(INCLUDES)

c1m2.out: $(OBJS)
	$(CC) $(CFLAGS) -o c1m2.out $(OBJS) $(INCLUDES) -Xlinker -Map=c1m2.map

# Compile all object files, but do not link.
.PHONY: compile-all
compile-all:
	make clean
	make $(OBJS)

# Compile all object files and link into a final executable.
.PHONY: build
build:
	make clean
	make c1m2.out

# Remove all compiled objects, preprocessed outputs, assembly outputs, executable files and build output files.
.PHONY: clean
clean:
	rm -f *.i *.o *.s *.map *.out