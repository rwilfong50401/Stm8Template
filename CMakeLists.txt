cmake_minimum_required(VERSION 3.25)
set(CMAKE_SYSTEM_NAME Generic)

#Find the compiler
find_program(SDCC_COMPILER sdcc)
if (${CMAKE_HOST_WIN32})
    if (NOT EXISTS "${SDCC_COMPILER}")
        list(APPEND CMAKE_PROGRAM_PATH "C:/Program Files(86)/SDCC/bin")
        find_program(SDCC_COMPILER sdcc)
    endif ()
    if (NOT EXISTS "${SDCC_COMPILER}")
        list(APPEND CMAKE_PROGRAM_PATH "C:/Program Files/SDCC/bin")
        find_program(SDCC_COMPILER sdcc)
    endif ()
endif ()
if (EXISTS "${SDCC_COMPILER}")
    set(CMAKE_C_COMPILER "${SDCC_COMPILER}")
else ()
    message(FATAL_ERROR "SDCC compiler is not found at your computer.")
endif ()

project(Stm8Template C)

# Verbose logging
set(CMAKE_VERBOSE_MAKEFILE ON)
set(LOG_VERBOSE true)

set(CMAKE_EXPORT_COMPILE_COMMANDS ON)
set(CMAKE_C_STANDARD 11)

add_compile_options(-mstm8 --debug --out-fmt-elf --opt-code-size)
add_link_options(-mstm8 --nostdlib --code-size 8192 --iram-size 1024 --out-fmt-elf -lstm8)

add_definitions(-DSTM8S103 -DUSE_STDPERIPH_DRIVER -DUSE_STDINT)

include_directories(PUBLIC ~/.local/share/sdcc/include)

add_subdirectory(lib/STM8_SPL)
add_subdirectory(MyProject)
add_subdirectory(SPLHeaderOnlyProject)
