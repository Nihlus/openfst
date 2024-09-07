set(OPENFST_CONST_LIBRARIES
    const8
    const16
    const64
)

add_library(fstconst)
set_target_properties(fstconst
    PROPERTIES
        VERSION 23.0.0
        SOVERSION 23
)

target_include_directories(fstconst
    PUBLIC
        $<BUILD_INTERFACE:${OPENFST_INCLUDE_DIR}>
        $<INSTALL_INTERFACE:include>
)

install(
    TARGETS fstconst
    EXPORT openfst
    LIBRARY DESTINATION ${CMAKE_INSTALL_LIBDIR}
    ARCHIVE DESTINATION ${CMAKE_INSTALL_LIBDIR}
    RUNTIME DESTINATION ${CMAKE_INSTALL_BINDIR}
    INCLUDES DESTINATION ${CMAKE_INSTALL_INCLUDEDIR}
)

foreach (CONST_LIBRARY IN LISTS OPENFST_CONST_LIBRARIES)
    add_library(${CONST_LIBRARY}-fst OBJECT)
    set_target_properties(${CONST_LIBRARY}-fst
        PROPERTIES
            VERSION 23.0.0
            SOVERSION 23
    )

    target_include_directories(${CONST_LIBRARY}-fst
        PUBLIC
            $<BUILD_INTERFACE:${OPENFST_INCLUDE_DIR}>
            $<INSTALL_INTERFACE:include>
    )

    target_sources(${CONST_LIBRARY}-fst
        PRIVATE
            ${CONST_LIBRARY}-fst.cc
    )

    target_link_options(${CONST_LIBRARY}-fst
        PRIVATE
            LINKER:-avoid-version
            LINKER:-module
    )

    target_link_libraries(${CONST_LIBRARY}-fst
        PUBLIC
            fst
    )

    install(
        TARGETS ${CONST_LIBRARY}-fst
        EXPORT openfst
        LIBRARY DESTINATION ${CMAKE_INSTALL_LIBDIR}
        ARCHIVE DESTINATION ${CMAKE_INSTALL_LIBDIR}
        RUNTIME DESTINATION ${CMAKE_INSTALL_BINDIR}
        INCLUDES DESTINATION ${CMAKE_INSTALL_INCLUDEDIR}
        FILE_SET HEADERS
    )

    target_link_libraries(fstconst
        PUBLIC
            ${CONST_LIBRARY}-fst
    )
endforeach ()