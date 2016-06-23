file( GLOB SRC *.c *.h *.cxx )

add_library(buddy STATIC ${SRC})
