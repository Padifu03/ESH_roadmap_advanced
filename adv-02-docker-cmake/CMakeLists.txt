cmake_minimum_required(VERSION 3.20)

project(blinky LANGUAGES C ASM)

set(TARGET_NAME blinky)
set(MCU cortex-m0plus)
set(LINKER_SCRIPT ${CMAKE_CURRENT_SOURCE_DIR}/stm32c031x6_flash.ld)

set(CMAKE_EXPORT_COMPILE_COMMANDS ON)

add_executable(${TARGET_NAME}
    src/main.c
    startup_stm32c031xx.S
)

set_target_properties(${TARGET_NAME} PROPERTIES SUFFIX ".elf")

target_include_directories(${TARGET_NAME} PRIVATE
    ${CMAKE_CURRENT_SOURCE_DIR}/inc
)

target_compile_options(${TARGET_NAME} PRIVATE
    -mcpu=${MCU}
    -mthumb
    -ffreestanding
    -fno-builtin
    -fdata-sections
    -ffunction-sections
    -Wall
    -Wextra
    -Werror
    $<$<COMPILE_LANGUAGE:C>:-std=c11>
    $<$<CONFIG:Debug>:-Og>
    $<$<CONFIG:Debug>:-g3>
    $<$<CONFIG:Release>:-O2>
    $<$<CONFIG:MinSizeRel>:-Os>
    $<$<CONFIG:RelWithDebInfo>:-O2>
    $<$<CONFIG:RelWithDebInfo>:-g3>
)

target_link_options(${TARGET_NAME} PRIVATE
    -T${LINKER_SCRIPT}
    -mcpu=${MCU}
    -mthumb
    -nostartfiles
    -nostdlib
    -Wl,--gc-sections
    -Wl,-Map=${CMAKE_CURRENT_BINARY_DIR}/${TARGET_NAME}.map
    -Wl,--entry=Reset_Handler
)

target_link_libraries(${TARGET_NAME} PRIVATE gcc)

add_custom_command(TARGET ${TARGET_NAME} POST_BUILD
    COMMAND ${CMAKE_OBJCOPY} -O ihex $<TARGET_FILE:${TARGET_NAME}> ${CMAKE_CURRENT_BINARY_DIR}/${TARGET_NAME}.hex
    COMMAND ${CMAKE_OBJCOPY} -O binary $<TARGET_FILE:${TARGET_NAME}> ${CMAKE_CURRENT_BINARY_DIR}/${TARGET_NAME}.bin
    COMMAND ${CMAKE_SIZE} --format=berkeley $<TARGET_FILE:${TARGET_NAME}>
    COMMENT "Generating HEX/BIN artifacts and size report"
)

find_program(OPENOCD_EXECUTABLE openocd)

if(OPENOCD_EXECUTABLE)
    add_custom_target(flash
        COMMAND ${OPENOCD_EXECUTABLE} -f interface/stlink.cfg -f target/stm32c0x.cfg -c "program $<TARGET_FILE:${TARGET_NAME}> verify reset exit"
        DEPENDS ${TARGET_NAME}
        USES_TERMINAL
        COMMENT "Flashing ${TARGET_NAME}.elf with OpenOCD"
    )
else()
    add_custom_target(flash
        COMMAND ${CMAKE_COMMAND} -E echo "OpenOCD was not found on PATH. Install OpenOCD and re-run 'cmake --preset <name>' before using the flash target."
        COMMAND ${CMAKE_COMMAND} -E false
        USES_TERMINAL
        COMMENT "OpenOCD is required for the flash target"
    )
    message(STATUS "OpenOCD not found. The 'flash' target will print a setup error until OpenOCD is installed and the project is reconfigured.")
endif()