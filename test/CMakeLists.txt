cmake_minimum_required(VERSION 3.0)

add_executable(bom-test bom-test.c)
target_link_libraries(bom-test ${LINK_TARGET})
configure_file(utf-8.txt utf-8.txt COPYONLY)
configure_file(utf-8-bom.txt utf-8-bom.txt COPYONLY)
add_test(NAME bom-test COMMAND bom-test WORKING_DIRECTORY ${CMAKE_CURRENT_BINARY_DIR})

add_executable(sample sample.c)
target_link_libraries(sample ${LINK_TARGET})
add_test(NAME sample COMMAND sample ${CMAKE_CURRENT_BINARY_DIR})

add_executable(sample1 sample1.c)
target_link_libraries(sample1 ${LINK_TARGET})
add_test(NAME sample1 COMMAND sample1 ${CMAKE_CURRENT_BINARY_DIR})

set_tests_properties(bom-test sample sample1 PROPERTIES ENVIRONMENT PATH=${CMAKE_BINARY_DIR}/src)
