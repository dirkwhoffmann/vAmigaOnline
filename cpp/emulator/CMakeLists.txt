# Usage: emcmake cmake -S . -B <builddir>

cmake_minimum_required(VERSION 3.16 FATAL_ERROR)

# Set the project name
project(vAmigaCore)

# Specify the C++ standard
set(CMAKE_CXX_STANDARD 20)
set(CMAKE_CXX_STANDARD_REQUIRED True)
set(CMAKE_CXX_EXTENSIONS False)

# Set the optimization level (if not specified on the command line)
if(NOT CMAKE_BUILD_TYPE)
#  set(CMAKE_BUILD_TYPE Release)
endif()

# Add the emulator library
add_library(vAmigaCore Amiga.cpp)

# Specify compile options
target_compile_definitions(vAmigaCore PUBLIC _USE_MATH_DEFINES)
target_compile_options(vAmigaCore PRIVATE -Wall -Werror)
target_compile_options(vAmigaCore PRIVATE -Wno-unused-parameter -Wno-unused-but-set-variable)
target_compile_options(vAmigaCore PRIVATE -O3 -mnontrapping-fptoint -fwasm-exceptions)
target_compile_options(vAmigaCore PRIVATE -Wfatal-errors)
target_compile_options(vAmigaCore PRIVATE -sWASM_WORKERS)
set_target_properties(vAmigaCore PROPERTIES LINK_FLAGS "-O1 -mnontrapping-fptoint -fwasm-exceptions" )

# Add include paths
target_include_directories(vAmigaCore PUBLIC
${CMAKE_CURRENT_SOURCE_DIR}/.
${CMAKE_CURRENT_SOURCE_DIR}/Utilities
${CMAKE_CURRENT_SOURCE_DIR}/Base
${CMAKE_CURRENT_SOURCE_DIR}/CPU
${CMAKE_CURRENT_SOURCE_DIR}/CPU/Moira
${CMAKE_CURRENT_SOURCE_DIR}/CPU/Moira/softfloat
${CMAKE_CURRENT_SOURCE_DIR}/CIA
${CMAKE_CURRENT_SOURCE_DIR}/Paula
${CMAKE_CURRENT_SOURCE_DIR}/Paula/Audio
${CMAKE_CURRENT_SOURCE_DIR}/Paula/DiskController
${CMAKE_CURRENT_SOURCE_DIR}/Paula/UART
${CMAKE_CURRENT_SOURCE_DIR}/Denise
${CMAKE_CURRENT_SOURCE_DIR}/Denise/Recorder
${CMAKE_CURRENT_SOURCE_DIR}/Agnus
${CMAKE_CURRENT_SOURCE_DIR}/Agnus/Sequencer
${CMAKE_CURRENT_SOURCE_DIR}/Agnus/Copper
${CMAKE_CURRENT_SOURCE_DIR}/Agnus/Blitter
${CMAKE_CURRENT_SOURCE_DIR}/Agnus/DmaDebugger
${CMAKE_CURRENT_SOURCE_DIR}/Memory
${CMAKE_CURRENT_SOURCE_DIR}/LogicBoard
${CMAKE_CURRENT_SOURCE_DIR}/Zorro
${CMAKE_CURRENT_SOURCE_DIR}/Peripherals
${CMAKE_CURRENT_SOURCE_DIR}/Peripherals/Drive
${CMAKE_CURRENT_SOURCE_DIR}/Peripherals/Mouse
${CMAKE_CURRENT_SOURCE_DIR}/Peripherals/Joystick
${CMAKE_CURRENT_SOURCE_DIR}/Peripherals/Keyboard
${CMAKE_CURRENT_SOURCE_DIR}/Media
${CMAKE_CURRENT_SOURCE_DIR}/Media/RomFiles
${CMAKE_CURRENT_SOURCE_DIR}/Media/DiskFiles
${CMAKE_CURRENT_SOURCE_DIR}/FileSystems
${CMAKE_CURRENT_SOURCE_DIR}/RetroShell
${CMAKE_CURRENT_SOURCE_DIR}/Misc
${CMAKE_CURRENT_SOURCE_DIR}/Misc/OSDebugger
${CMAKE_CURRENT_SOURCE_DIR}/Misc/RemoteServers
${CMAKE_CURRENT_SOURCE_DIR}/Misc/RegressionTester
${CMAKE_CURRENT_SOURCE_DIR}/xdms)

# Add sub directories
add_subdirectory(Utilities)
add_subdirectory(Base)
add_subdirectory(CPU)
add_subdirectory(CIA)
add_subdirectory(Paula)
add_subdirectory(Denise)
add_subdirectory(Agnus)
add_subdirectory(Memory)
add_subdirectory(LogicBoard)
add_subdirectory(Zorro)
add_subdirectory(Peripherals)
add_subdirectory(Media)
add_subdirectory(FileSystems)
add_subdirectory(RetroShell)
add_subdirectory(Misc)
add_subdirectory(xdms)
add_subdirectory(CPU/Moira/softfloat)

# Add libraries
target_link_libraries(vAmigaCore xdms softfloat)