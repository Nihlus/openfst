set(OPENFST_SPECIAL_LIBRARIES
    phi
    rho
    sigma
)

add_library(fstspecial)

set_target_properties(fstspecial
    PROPERTIES
        VERSION 23.0.0
        SOVERSION 23
)

target_compile_features(fstspecial
        PUBLIC
        cxx_std_17
)

target_include_directories(fstspecial
    PUBLIC
        $<BUILD_INTERFACE:${OPENFST_INCLUDE_DIR}>
        $<INSTALL_INTERFACE:include>
)

install(
    TARGETS fstspecial
    EXPORT openfst
    LIBRARY DESTINATION ${CMAKE_INSTALL_LIBDIR}
    ARCHIVE DESTINATION ${CMAKE_INSTALL_LIBDIR}
    RUNTIME DESTINATION ${CMAKE_INSTALL_BINDIR}
    INCLUDES DESTINATION ${CMAKE_INSTALL_INCLUDEDIR}
    FILE_SET HEADERS
)

foreach (SPECIAL_LIBRARY IN LISTS OPENFST_SPECIAL_LIBRARIES)
    add_library(${SPECIAL_LIBRARY}-fst ${OPENFST_OBJECT_LIBRARY_TYPE})

    set_target_properties(${SPECIAL_LIBRARY}-fst
        PROPERTIES
            VERSION 23.0.0
            SOVERSION 23
    )

    target_compile_features(${SPECIAL_LIBRARY}-fst
            PUBLIC
            cxx_std_17
    )

    target_include_directories(${SPECIAL_LIBRARY}-fst
        PUBLIC
            $<BUILD_INTERFACE:${OPENFST_INCLUDE_DIR}>
            $<INSTALL_INTERFACE:include>
    )

    target_sources(${SPECIAL_LIBRARY}-fst
        PRIVATE
            ${SPECIAL_LIBRARY}-fst.cc
        PUBLIC FILE_SET HEADERS
        BASE_DIRS ${OPENFST_INCLUDE_DIR}
        FILES
            ${OPENFST_INCLUDE_DIR}/fst/extensions/special/${SPECIAL_LIBRARY}-fst.h
    )

    target_link_options(${SPECIAL_LIBRARY}-fst
        PRIVATE
            LINKER:-avoid-version
            LINKER:-module
    )

    target_link_libraries(${SPECIAL_LIBRARY}-fst
        PUBLIC
            fst
    )

    install(
        TARGETS ${SPECIAL_LIBRARY}-fst
        EXPORT openfst
        LIBRARY DESTINATION ${CMAKE_INSTALL_LIBDIR}
        ARCHIVE DESTINATION ${CMAKE_INSTALL_LIBDIR}
        RUNTIME DESTINATION ${CMAKE_INSTALL_BINDIR}
        INCLUDES DESTINATION ${CMAKE_INSTALL_INCLUDEDIR}
        FILE_SET HEADERS
    )

    target_link_libraries(fstspecial
        PUBLIC
            ${SPECIAL_LIBRARY}-fst
    )
endforeach ()

if (OPENFST_BIN)
    add_executable(fstspecial-bin)

    target_compile_features(fstspecial-bin
        PUBLIC
            cxx_std_17
    )

    set_target_properties(fstspecial-bin
        PROPERTIES
            OUTPUT_NAME fstspecial
    )

    target_sources(fstspecial-bin
        PRIVATE
            fstspecial.cc
    )

    target_link_libraries(fstspecial-bin
        PRIVATE
            fstspecial
            fstscript
    )

    target_include_directories(fstspecial-bin
        PRIVATE
            ${OPENFST_BIN_SOURCE_DIR}
    )

    install(
        TARGETS fstspecial-bin
        RUNTIME DESTINATION ${CMAKE_INSTALL_BINDIR}
    )
endif ()
