file(GLOB SRC_FILES "*.c" "./real/*.c")
list(REMOVE_ITEM SRC_FILES "${CMAKE_CURRENT_SOURCE_DIR}/real/polyphase.c")

# Determine the assembly files based on the toolchain
if(CMAKE_C_COMPILER_ID STREQUAL "GNU")
    set(ASM_FILES "./real/arm/asmmisc_gcc.s" "./real/arm/asmpoly_thumb2_gcc.s")
elseif(CMAKE_C_COMPILER_ID STREQUAL "ARMCC")
    set(ASM_FILES "./real/arm/asmmisc_rvds.s" "./real/arm/asmpoly_thumb2_rvds.s")
elseif(CMAKE_C_COMPILER_ID STREQUAL "IAR")
    set(ASM_FILES "./real/arm/asmmisc_iar.s" "./real/arm/asmpoly_thumb2_iar.s")
else()
    message(FATAL_ERROR "Unsupported toolchain")
endif()

# Combine C and assembly source files
list(APPEND SRC_FILES ${ASM_FILES})

# Define the include directories
include_directories("${CMAKE_CURRENT_SOURCE_DIR}/pub" "${CMAKE_CURRENT_SOURCE_DIR}/real")

# Add the library
add_library(helix STATIC
    ${SRC_FILES}
)

target_include_directories(helix INTERFACE
    ${CMAKE_CURRENT_LIST_DIR}
    ${CMAKE_CURRENT_SOURCE_DIR}/pub
    ${CMAKE_CURRENT_SOURCE_DIR}/real
)