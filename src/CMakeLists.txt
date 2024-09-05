set(OPENFST_INCLUDE_DIR ${CMAKE_CURRENT_LIST_DIR}/include)
set(OPENFST_BIN_SOURCE_DIR ${CMAKE_CURRENT_LIST_DIR}/bin)

add_subdirectory(lib)

if (OPENFST_BIN)
    add_subdirectory(script)
    add_subdirectory(bin)
endif ()

include(CTest)
if (BUILD_TESTING)
    add_subdirectory(test)
endif ()

add_subdirectory(extensions)
