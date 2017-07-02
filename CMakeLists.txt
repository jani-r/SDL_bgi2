project (SDL_bgi2)
cmake_minimum_required (VERSION 3.7)

if(NOT MSVC)
	message( FATAL_ERROR "This CMakeFile is only tested with MSVC." )
endif()

# Location for SDL2 library and include files
if(EXISTS ${CMAKE_SOURCE_DIR}/SDL2)
	set(sdl2_library_dir ${CMAKE_SOURCE_DIR}/SDL2)
else()
	message( FATAL_ERROR "SDL2 development libraries not found. Add SDL2 library and include files in folder SDL2." )
endif()

# Needed configuration for SDL2 
add_library(sdl2dll STATIC IMPORTED)
add_library(sdl2lib STATIC IMPORTED)
add_library(sdl2main STATIC IMPORTED)
add_dependencies(sdl2lib sdl2dll)
add_dependencies(sdl2main sdl2dll)
set_target_properties(sdl2dll PROPERTIES IMPORTED_LOCATION ${sdl2_library_dir}/bin/SDL2.dll)
set_target_properties(sdl2lib PROPERTIES IMPORTED_LOCATION ${sdl2_library_dir}/lib/SDL2.lib)
set_target_properties(sdl2main PROPERTIES IMPORTED_LOCATION ${sdl2_library_dir}/lib/SDL2main.lib)
set(sdl_include_dirs ${sdl2_library_dir}/include)

# Set general output directories
set( CMAKE_RUNTIME_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/bin )
set( CMAKE_LIBRARY_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/lib )
set( CMAKE_ARCHIVE_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/lib )

# Add library SDL_bgi2
set(targetname SDL_bgi2)
add_library(${targetname} STATIC src/SDL_bgi.c)
target_include_directories(${targetname} PRIVATE ${sdl_include_dirs})
target_link_libraries(${targetname} sdl2 sdl2main)

# Set output directories for target
set_target_properties( ${targetname} PROPERTIES RUNTIME_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/_output/bin )
set_target_properties( ${targetname} PROPERTIES RUNTIME_OUTPUT_DIRECTORY_DEBUG ${CMAKE_BINARY_DIR}/_output/bin )
set_target_properties( ${targetname} PROPERTIES RUNTIME_OUTPUT_DIRECTORY_RELEASE ${CMAKE_BINARY_DIR}/_output/bin )

set_target_properties( ${targetname} PROPERTIES LIBRARY_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/_output/lib )
set_target_properties( ${targetname} PROPERTIES LIBRARY_OUTPUT_DIRECTORY_DEBUG ${CMAKE_BINARY_DIR}/_output/lib )
set_target_properties( ${targetname} PROPERTIES LIBRARY_OUTPUT_DIRECTORY_RELEASE ${CMAKE_BINARY_DIR}/_output/lib )

set_target_properties( ${targetname} PROPERTIES ARCHIVE_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/_output/lib )
set_target_properties( ${targetname} PROPERTIES ARCHIVE_OUTPUT_DIRECTORY_DEBUG ${CMAKE_BINARY_DIR}/_output/lib )
set_target_properties( ${targetname} PROPERTIES ARCHIVE_OUTPUT_DIRECTORY_RELEASE ${CMAKE_BINARY_DIR}/_output/lib )

# Copy graphics.h to output
add_custom_command(
        TARGET ${targetname} POST_BUILD
        COMMAND ${CMAKE_COMMAND} -E copy
                ${CMAKE_SOURCE_DIR}/src/graphics.h
                ${CMAKE_BINARY_DIR}/_output/include/SDL_bgi/graphics.h)

# Copy SDL_bgi.h to output
add_custom_command(
        TARGET ${targetname} POST_BUILD
        COMMAND ${CMAKE_COMMAND} -E copy
                ${CMAKE_SOURCE_DIR}/src/SDL_bgi.h
                ${CMAKE_BINARY_DIR}/_output/include/SDL_bgi/SDL_bgi.h)

# Copy SDL_bgi_font.h to output
add_custom_command(
        TARGET ${targetname} POST_BUILD
        COMMAND ${CMAKE_COMMAND} -E copy
                ${CMAKE_SOURCE_DIR}/src/SDL_bgi_font.h
                ${CMAKE_BINARY_DIR}/_output/include/SDL_bgi/SDL_bgi_font.h)

# Copy SDL2 include files to output
add_custom_command(
        TARGET ${targetname} POST_BUILD
        COMMAND ${CMAKE_COMMAND} -E copy_directory
                ${sdl2_library_dir}/include
                ${CMAKE_BINARY_DIR}/_output/include/SDL2)

				
# Copy SDL2.dll to output
add_custom_command(
        TARGET ${targetname} POST_BUILD
        COMMAND ${CMAKE_COMMAND} -E copy
                ${sdl2_library_dir}/bin/SDL2.dll
                ${CMAKE_BINARY_DIR}/_output/bin/SDL2.dll)
				
# Copy SDL2.lib to output
add_custom_command(
        TARGET ${targetname} POST_BUILD
        COMMAND ${CMAKE_COMMAND} -E copy
                ${sdl2_library_dir}/lib/SDL2.lib
                ${CMAKE_BINARY_DIR}/_output/lib/SDL2.lib)

# Copy SDL2main.lib to output
add_custom_command(
        TARGET ${targetname} POST_BUILD
        COMMAND ${CMAKE_COMMAND} -E copy
                ${sdl2_library_dir}/lib/SDL2main.lib
                ${CMAKE_BINARY_DIR}/_output/lib/SDL2main.lib)
				