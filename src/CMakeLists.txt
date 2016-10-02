file( GLOB SRC *.c *.h *.cxx )

add_library(buddy_static STATIC ${SRC})
add_library(buddy_shared SHARED ${SRC})
