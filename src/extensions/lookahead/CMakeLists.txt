set(OPENFST_LOOKAHEAD_LIBRARIES
    arc_lookahead
    ilabel_lookahead
    olabel_lookahead
)

add_library(fstlookahead)

set_target_properties(fstlookahead
    PROPERTIES
        VERSION 23.0.0
        SOVERSION 23
)

target_compile_features(fstlookahead
        PUBLIC
        cxx_std_17
)

target_include_directories(fstlookahead
    PUBLIC
        $<BUILD_INTERFACE:${OPENFST_INCLUDE_DIR}>
        $<INSTALL_INTERFACE:include>
)

foreach (LOOKAHEAD_LIBRARY IN LISTS OPENFST_LOOKAHEAD_LIBRARIES)
    if (OPENFST_SUPPORTS_OBJECT_LIBRARIES)
        add_library(${LOOKAHEAD_LIBRARY}-fst OBJECT)

        set_target_properties(${LOOKAHEAD_LIBRARY}-fst
            PROPERTIES
                VERSION 23.0.0
                SOVERSION 23
        )

        target_compile_features(${LOOKAHEAD_LIBRARY}-fst
            PUBLIC
                cxx_std_17
        )

        target_include_directories(${LOOKAHEAD_LIBRARY}-fst
            PUBLIC
                $<BUILD_INTERFACE:${OPENFST_INCLUDE_DIR}>
                $<INSTALL_INTERFACE:include>
        )

        target_sources(${LOOKAHEAD_LIBRARY}-fst
            PRIVATE
                ${LOOKAHEAD_LIBRARY}-fst.cc
        )

        target_link_options(${LOOKAHEAD_LIBRARY}-fst
            PRIVATE
                LINKER:-avoid-version
                LINKER:-module
        )

        target_link_libraries(${LOOKAHEAD_LIBRARY}-fst
            PUBLIC
                fst
        )

        install(
            TARGETS ${LOOKAHEAD_LIBRARY}-fst
            EXPORT openfst
            LIBRARY DESTINATION ${CMAKE_INSTALL_LIBDIR}
            ARCHIVE DESTINATION ${CMAKE_INSTALL_LIBDIR}
            RUNTIME DESTINATION ${CMAKE_INSTALL_BINDIR}
            INCLUDES DESTINATION ${CMAKE_INSTALL_INCLUDEDIR}
            FILE_SET HEADERS
        )

        target_link_libraries(fstlookahead
            PUBLIC
                ${LOOKAHEAD_LIBRARY}-fst
        )
    else ()
        target_sources(fstlookahead
            PRIVATE
                ${LOOKAHEAD_LIBRARY}-fst.cc
        )
    endif ()
endforeach ()

install(
    TARGETS fstlookahead
    EXPORT openfst
    LIBRARY DESTINATION ${CMAKE_INSTALL_LIBDIR}
    ARCHIVE DESTINATION ${CMAKE_INSTALL_LIBDIR}
    RUNTIME DESTINATION ${CMAKE_INSTALL_BINDIR}
    INCLUDES DESTINATION ${CMAKE_INSTALL_INCLUDEDIR}
    FILE_SET HEADERS
)
