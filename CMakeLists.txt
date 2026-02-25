project (cppgtfs)

set(CMAKE_C_STANDARD 17)
set(CMAKE_CXX_STANDARD 17)

if (!LIBZIP_FOUND)
    find_package(LibZip)
endif()

if (LIBZIP_FOUND)
	set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -DLIBZIP_FOUND=1")
endif()

# Gather source files from src/ so this directory builds into object files only.
file(GLOB_RECURSE CPPGTFS_SOURCES
    ${CMAKE_CURRENT_SOURCE_DIR}/src/*.c
    ${CMAKE_CURRENT_SOURCE_DIR}/src/*.cc
    ${CMAKE_CURRENT_SOURCE_DIR}/src/*.cpp
    ${CMAKE_CURRENT_SOURCE_DIR}/src/*.cxx
)

# Build an OBJECT library whose object files can be linked into a single library later.
add_library(cppgtfs_objects OBJECT ${CPPGTFS_SOURCES})
set_target_properties(cppgtfs_objects PROPERTIES POSITION_INDEPENDENT_CODE ON)

# Ensure consumers can include headers from src/
target_include_directories(cppgtfs_objects PUBLIC ${CMAKE_CURRENT_SOURCE_DIR}/src)

if (LIBZIP_FOUND)
    target_compile_definitions(cppgtfs_objects PUBLIC LIBZIP_FOUND=1)
endif()
