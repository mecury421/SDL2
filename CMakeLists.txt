cmake_minimum_required(VERSION 3.0 FATAL_ERROR)
project(SDL C)

# define the correct library root for the detected platform
if (WIN32)
    if (CMAKE_SIZEOF_VOID_P EQUAL 8)
        set (SDL_LIB_ROOT "${CMAKE_CURRENT_SOURCE_DIR}/lib/x64")
    else ()
        set (SDL_LIB_ROOT "${CMAKE_CURRENT_SOURCE_DIR}/lib/x86")
    endif ()
else (WIN32)
    message(FATAL_ERROR "Platform not supported")
endif (WIN32)

# for use by the calling project, used to automate copying of the DLL to the correct place
set(SDL_DLL_PATH "${SDL_LIB_ROOT}/SDL2.dll" CACHE STRING "" FORCE)

# define the interface library (to emulate a compiled library)
add_library(sdl2 INTERFACE)
target_link_libraries(sdl2 INTERFACE "${SDL_LIB_ROOT}/SDL2.lib" "${SDL_LIB_ROOT}/SDL2test.lib")
target_include_directories(sdl2 INTERFACE "${CMAKE_CURRENT_SOURCE_DIR}/include")

# define the optional main library
add_library(sdl2main INTERFACE)
target_link_libraries(sdl2main INTERFACE "${SDL_LIB_ROOT}/SDL2main.lib")
