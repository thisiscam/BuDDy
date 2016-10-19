include(GenerateExportHeader)

file( GLOB SRC *.c *.h *.cxx )

add_library(buddy_static STATIC ${SRC})
add_library(buddy_shared SHARED ${SRC})

generate_export_header(buddy_shared)
target_include_directories(buddy_shared PUBLIC ${CMAKE_CURRENT_BINARY_DIR})
target_include_directories(buddy_static PUBLIC ${CMAKE_CURRENT_BINARY_DIR})
set_target_properties(buddy_static PROPERTIES
  COMPILE_FLAGS -DBUDDY_SHARED_AND_STATIC_STATIC_DEFINE)

set_target_properties(buddy_shared buddy_static PROPERTIES WINDOWS_EXPORT_ALL_SYMBOLS TRUE)

configure_file(config.h.in config.h @ONLY)

OPTION(BUDDY_MULTITHREAD_REFCOUNT 
	  "Build with multithreaded ref counting, useful for binding languages with multithreaded Garbage Collection." 
	  OFF)

if(BUDDY_MULTITHREAD_REFCOUNT)
	target_compile_definitions(buddy_static PRIVATE -DMULTITHREAD_REFCOUNT)
	target_compile_definitions(buddy_shared PRIVATE -DMULTITHREAD_REFCOUNT)
endif()
