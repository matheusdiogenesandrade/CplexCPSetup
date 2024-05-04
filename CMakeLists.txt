cmake_minimum_required(VERSION 3.23)
project(CplexCP)

##### SETs
set (Project_PATH "${PROJECT_SOURCE_DIR}")
set (CMAKE_MODULE_PATH ${CMAKE_MODULE_PATH} ${CMAKE_CURRENT_SOURCE_DIR}/support/cmake)
set (CMAKE_BUILD_TYPE Release) 
set (CMAKE_CXX_STANDARD 20)
set (BUILD_SHARED_LIBS On)

##### Packages
find_package (CPLEX)

##### Include Dirs 

link_directories(${CPLEX_CP_LIBRARY})

include_directories(
    SYSTEM
    includes
    "${PROJECT_BINARY_DIR}"
    "${CPLEX_CONCERT_INCLUDE_DIR}"
    "${CPLEX_ILOCPLEX_INCLUDE_DIR}"
    "${CPLEX_CP_INCLUDE_DIR}"
    )


##### Compile options 

add_compile_options(-Wall
    #
    -DIL_STD
    -Wfatal-errors
    -pedantic
    -fopenmp
    -DDEBUG
    -fdiagnostics-color=always
    -lz
    -std=c++20
    # CPLEX
    -lconcert 
    -lilocplex 
    -lcplex 
    -lcplex-cp
    -lcplex-concert
    -lcplex-library
    #
    -lm 
    -lpthread 
    -ldl
    -Wno-sign-compare
    )

file(GLOB_RECURSE SOURCES "src/*.cpp")

add_executable(
    main 
    ${SOURCES} 
    )

target_link_libraries(
    main 
    ${CPLEX_CONCERT_LIBRARY}
    ${CPLEX_ILOCPLEX_LIBRARY}
    ${CPLEX_LIBRARY}
    ${CPLEX_CP_LIBRARY}
    ${CPLEX_CP_LIBRARY_DEBUG}
    pthread 
    m 
    dl
    )
