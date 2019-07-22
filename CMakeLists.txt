#
# example of CMake from:
# https://the-missing-bit.com/post/2018-07-11-avr-cmake/

cmake_minimum_required(VERSION 3.0)

# Project name
project(miniAVRfreeRTOS C)

# Product filename
set(PRODUCT_NAME my_firmware)
set(CMAKE_C_STANDARD 11)

## AVR Chip Configuration
# 16Mhz, this should match the crystal on your board,
# I use 16Mhz and 3.3V for the lowest power consumption
set(F_CPU 16000000UL)

# CPU, you can find the list here:
# https://gcc.gnu.org/onlinedocs/gcc/AVR-Options.html
set(MCU atmega2560)

# Default Baudrate for UART, read avr include/util/setbaud.h for usage
set(BAUD 115200)

# avrdude -c "?" for the list of programmers
set(PROG_TYPE wiring)

# disable auto adding unsupported flags 
SET(CMAKE_SHARED_LIBRARY_LINK_C_FLAGS)

# Use AVR GCC toolchain
set(CMAKE_SYSTEM_NAME Generic)
set(CMAKE_CXX_COMPILER avr-g++)
set(CMAKE_C_COMPILER avr-gcc)
set(CMAKE_ASM_COMPILER avr-gcc)

# AVR Fuses, must be in concordance with your hardware and F_CPU
# http://eleccelerator.com/fusecalc/fusecalc.php?chip=atmega328p
#set(E_FUSE 0xfd)
set(H_FUSE 0xD8)
set(L_FUSE 0xFF)
#set(LOCK_BIT 0xff)

# Pass defines to compiler
add_definitions(
        -DF_CPU=${F_CPU}
        -DBAUD=${BAUD}
        -DARDUINO=10809 
        -DARDUINO_AVR_MEGA2560 
        -DARDUINO_ARCH_AVR
)
# mmcu MUST be passed to bot the compiler and linker, this handle the linker
set(CMAKE_EXE_LINKER_FLAGS -mmcu=${MCU})

set(DEBUG_LEVEL -g)
set(WARNINGS -Wall -Wextra -Wshadow -Wpointer-arith -Wbad-function-cast -Wcast-align -Wsign-compare -Waggregate-return -Wstrict-prototypes -Wmissing-prototypes -Wmissing-declarations -Wunused -Wpedantic)


add_compile_options(
        -mmcu=${MCU} # MCU
        -std=gnu11 # C11 standard
        -Os # optimize
        ${DEBUG_LEVEL}
        ${WARNINGS}
)

file(GLOB SRC_FILES "src/*.*") # Load all files in src folder

# Create one target
add_executable(${PRODUCT_NAME} ${SRC_FILES})

# Rename the output to .elf as we will create multiple files
set_target_properties(${PRODUCT_NAME} PROPERTIES OUTPUT_NAME ${PRODUCT_NAME}.elf)

# Strip binary for upload
add_custom_target(strip ALL avr-strip ${PRODUCT_NAME}.elf DEPENDS ${PRODUCT_NAME})

# Transform binary into hex file, we ignore the eeprom segments in the step
add_custom_target(hex ALL avr-objcopy -R .eeprom -O ihex ${PRODUCT_NAME}.elf ${PRODUCT_NAME}.hex DEPENDS strip)

# Transform binary into hex file, this is the eeprom part (empty if you don't
# use eeprom static variables)
add_custom_target(eeprom avr-objcopy -j .eeprom  --set-section-flags=.eeprom="alloc,load"  --change-section-lma .eeprom=0 -O ihex ${PRODUCT_NAME}.elf ${PRODUCT_NAME}.eep DEPENDS strip)

# calculate size of program
add_custom_target(size avr-size -A $TEMPBUILD/RGB_LED.ino.elf)

# Upload the firmware with avrdude
add_custom_target(upload avrdude  -c${PROG_TYPE} -p${MCU}  -P/dev/ttyACM0 -b${BAUD} -v -D -Uflash:w:${PRODUCT_NAME}.hex:i)

# Upload the eeprom with avrdude
add_custom_target(upload_eeprom avrdude -c ${PROG_TYPE} -p ${MCU}  -U eeprom:w:${PRODUCT_NAME}.eep DEPENDS eeprom)

# Burn fuses
#add_custom_target(fuses avrdude -c ${PROG_TYPE} -p${MCU}  -U lfuse:w:${L_FUSE}:m -U hfuse:w:${H_FUSE}:m -U efuse:w:${E_FUSE}:m -U lock:w:${LOCK_BIT}:m )


# Clean extra files
set_directory_properties(PROPERTIES ADDITIONAL_MAKE_CLEAN_FILES "${PRODUCT_NAME}.hex;${PRODUCT_NAME}.eeprom;${PRODUCT_NAME}.lst")
