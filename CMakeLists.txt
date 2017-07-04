project (SDL_bgi2)
cmake_minimum_required (VERSION 3.7)

if(MSVC)
	#message( FATAL_ERROR "This CMakeFile is only tested with MSVC." )
	message( "Using MSVC" )
elseif(MINGW)
	message( "Using MINGW" )
else()
	message( FATAL_ERROR "This CMakeFile is only tested with MSVC and MINGW." )
endif()

# Location for SDL2 library and include files
if(EXISTS ${CMAKE_SOURCE_DIR}/SDL2)
	set(sdl2_library_dir ${CMAKE_SOURCE_DIR}/SDL2)
	set(sdl_include_dirs ${sdl2_library_dir}/include)
else()
	message( FATAL_ERROR "SDL2 development libraries not found. Add SDL2 library and include files in folder SDL2." )
endif()

# Set general output directories
set( CMAKE_RUNTIME_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/bin )
set( CMAKE_LIBRARY_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/lib )
set( CMAKE_ARCHIVE_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/lib )

# Add library SDL_bgi
set(targetname SDL_bgi)
add_library(${targetname} STATIC src/SDL_bgi.c)
target_include_directories(${targetname} PRIVATE ${CMAKE_SOURCE_DIR}/src ${sdl_include_dirs})

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
if(MSVC)
	add_custom_command(
			TARGET ${targetname} POST_BUILD
			COMMAND ${CMAKE_COMMAND} -E copy
					${sdl2_library_dir}/bin/SDL2.dll
					${CMAKE_BINARY_DIR}/_output/bin/SDL2.dll)
elseif(MINGW)
	add_custom_command(
			TARGET ${targetname} POST_BUILD
			COMMAND ${CMAKE_COMMAND} -E copy
					${sdl2_library_dir}/bin/libSDL2.dll
					${CMAKE_BINARY_DIR}/_output/bin/libSDL2.dll)
endif()
				
# Copy SDL2.lib to output
if(MSVC)
	add_custom_command(
			TARGET ${targetname} POST_BUILD
			COMMAND ${CMAKE_COMMAND} -E copy
					${sdl2_library_dir}/lib/SDL2.lib
					${CMAKE_BINARY_DIR}/_output/lib/SDL2.lib)
elseif(MINGW)
	add_custom_command(
			TARGET ${targetname} POST_BUILD
			COMMAND ${CMAKE_COMMAND} -E copy
					${sdl2_library_dir}/lib/libSDL2.a
					${CMAKE_BINARY_DIR}/_output/lib/libSDL2.a)
endif()

# Copy SDL2main.lib to output
if(MSVC)
	add_custom_command(
			TARGET ${targetname} POST_BUILD
			COMMAND ${CMAKE_COMMAND} -E copy
					${sdl2_library_dir}/lib/SDL2main.lib
					${CMAKE_BINARY_DIR}/_output/lib/SDL2main.lib)
elseif(MINGW)
	add_custom_command(
			TARGET ${targetname} POST_BUILD
			COMMAND ${CMAKE_COMMAND} -E copy
					${sdl2_library_dir}/lib/libSDL2main.a
					${CMAKE_BINARY_DIR}/_output/lib/libSDL2main.a)
endif()
				
				
				
project (SDL_bgi_sample_mandelbrot)

set(targetname mandelbrot)
add_executable(${targetname} ${CMAKE_SOURCE_DIR}/test/mandelbrot.c)
target_include_directories(${targetname} PRIVATE ${CMAKE_SOURCE_DIR}/src ${sdl_include_dirs})
if(MSVC)
	target_link_libraries(${targetname} SDL_bgi ${sdl2_library_dir}/lib/SDL2.lib ${sdl2_library_dir}/lib/SDL2main.lib)
elseif(MINGW)
	target_link_libraries(${targetname} SDL_bgi ${sdl2_library_dir}/lib/libSDL2.dll.a ${sdl2_library_dir}/lib/libSDL2.a ${sdl2_library_dir}/lib/libSDL2.a ${sdl2_library_dir}/lib/libSDL2main.a)
endif()

# Set output directories for target
set_target_properties( ${targetname} PROPERTIES RUNTIME_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/_output/bin )
set_target_properties( ${targetname} PROPERTIES RUNTIME_OUTPUT_DIRECTORY_DEBUG ${CMAKE_BINARY_DIR}/_output/bin )
set_target_properties( ${targetname} PROPERTIES RUNTIME_OUTPUT_DIRECTORY_RELEASE ${CMAKE_BINARY_DIR}/_output/bin )

# Copy SDL2.dll to output
if(MSVC)
	add_custom_command(
			TARGET ${targetname} POST_BUILD
			COMMAND ${CMAKE_COMMAND} -E copy
					${sdl2_library_dir}/lib/SDL2.lib
					${CMAKE_BINARY_DIR}/_output/lib/SDL2.lib)
elseif(MINGW)
	add_custom_command(
			TARGET ${targetname} POST_BUILD
			COMMAND ${CMAKE_COMMAND} -E copy
					${sdl2_library_dir}/lib/libSDL2.a
					${CMAKE_BINARY_DIR}/_output/lib/libSDL2.a)
endif()		