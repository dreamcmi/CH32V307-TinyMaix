set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -O3 " CACHE STRING "c flags")
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -O3 " CACHE STRING "c++ flags")

add_library(tinymaix)
include_directories(.)
target_sources(tinymaix
                PRIVATE 
                    tm_stat.c
                    tm_model.c
                    tm_layers.c
                    tm_layers_O1.c)

target_link_libraries(tinymaix m)

