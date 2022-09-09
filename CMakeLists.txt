set(CMAKE_SYSTEM_NAME Generic)
set(CMAKE_TRY_COMPILE_TARGET_TYPE STATIC_LIBRARY)
cmake_minimum_required(VERSION 3.20)

set(TOOLPATH riscv-none-embed-)
set(CMAKE_C_COMPILER ${TOOLPATH}gcc)
set(CMAKE_CXX_COMPILER ${TOOLPATH}g++)
set(CMAKE_ASM_COMPILER ${TOOLPATH}gcc)
set(CMAKE_AR ${TOOLPATH}ar)
set(CMAKE_OBJCOPY ${TOOLPATH}objcopy)
set(CMAKE_OBJDUMP ${TOOLPATH}objdump)
set(SIZE ${TOOLPATH}size)

project(CH32V307-TinyMaix C CXX ASM)
set(CMAKE_CXX_STANDARD 11)
set(CMAKE_C_STANDARD 99)

add_compile_options(-march=rv32imacxw -mabi=ilp32 -mcmodel=medany -msmall-data-limit=8 -mno-save-restore)
add_compile_options(-fmessage-length=0 -fsigned-char -ffunction-sections -fdata-sections -fno-common )
add_link_options(-nostartfiles -Xlinker --gc-sections -Wl,--print-memory-usage -Wl,-Map,${PROJECT_NAME}.map --specs=nano.specs --specs=nosys.specs)
add_link_options(-T ${CMAKE_SOURCE_DIR}/Ld/Link.ld)

add_compile_options(-O3)
# add_compile_options(-Wall)


include_directories(APP
                    Core
                    Debug 
                    Peripheral/inc
                    TinyMaix)


file(GLOB_RECURSE SOURCES 
                "APP/*.c"
                "Core/core_riscv.c" 
                "Debug/debug.c"
                "Peripheral/SRC/*.c" 
                "Startup/startup_ch32v30x_D8C.S"
                )

add_subdirectory(TinyMaix)

add_executable(${PROJECT_NAME}.elf ${SOURCES})

target_link_libraries(${PROJECT_NAME}.elf printfloat m tinymaix)

set(HEX_FILE ${PROJECT_BINARY_DIR}/${PROJECT_NAME}.hex)
set(BIN_FILE ${PROJECT_BINARY_DIR}/${PROJECT_NAME}.bin)
set(LST_FILE ${PROJECT_BINARY_DIR}/${PROJECT_NAME}.lst)
add_custom_command(TARGET ${PROJECT_NAME}.elf POST_BUILD
        COMMAND ${CMAKE_OBJCOPY} -Oihex $<TARGET_FILE:${PROJECT_NAME}.elf> ${HEX_FILE}
        COMMAND ${CMAKE_OBJCOPY} -Obinary $<TARGET_FILE:${PROJECT_NAME}.elf> ${BIN_FILE}
        COMMAND ${CMAKE_OBJDUMP} --all-headers --demangle --disassemble $<TARGET_FILE:${PROJECT_NAME}.elf> > ${LST_FILE}
        COMMAND ${SIZE} --format=berkeley $<TARGET_FILE:${PROJECT_NAME}.elf>
)
