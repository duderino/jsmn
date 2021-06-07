cmake_minimum_required(VERSION 3.11)
project(jsmn VERSION ${VERSION} LANGUAGES C)
enable_testing()

macro(add_flavor NAME OPTS)
    # Lib
    add_library(${NAME} jsmn.c)
    target_include_directories (${NAME} PUBLIC ${CMAKE_CURRENT_SOURCE_DIR})
    target_compile_options(${NAME} PUBLIC ${C_FLAGS} ${OPTS})
    # Test executable
    set(TEST_NAME "${NAME}_test")
    add_executable(${TEST_NAME} test/tests.c)
    target_compile_options(${TEST_NAME} PUBLIC ${C_FLAGS} ${OPTS})
    target_link_libraries(${TEST_NAME} ${NAME})
    target_include_directories(${TEST_NAME} PRIVATE ${CMAKE_CURRENT_SOURCE_DIR})
    add_test(NAME ${TEST_NAME} COMMAND ${TEST_NAME} WORKING_DIRECTORY ${PROJECT_SOURCE_DIR}/test)
endmacro()

add_flavor(jsmn_strict_parent_links "-DJSMN_STRICT=1 -DJSMN_PARENT_LINKS=1")
add_flavor(jsmn_strict "-DJSMN_STRICT=1")
add_flavor(jsmn_parent_links "-DJSMN_PARENT_LINKS=1")
add_flavor(jsmn "")
