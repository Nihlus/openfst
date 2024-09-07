set(OPENFST_LINEAR_LIBRARIES
    linear-tagger
    linear-classifier
)

set(OPENFST_LINEAR_PROGRAMS
    fstlinear
    fstloglinearapply
)

foreach (LINEAR_LIBRARY IN LISTS OPENFST_LINEAR_LIBRARIES)
    add_library(${LINEAR_LIBRARY}-fst)

    set_target_properties(${LINEAR_LIBRARY}-fst
        PROPERTIES
            VERSION 23.0.0
            SOVERSION 23
    )

    target_sources(${LINEAR_LIBRARY}-fst
        PRIVATE
            ${LINEAR_LIBRARY}-fst.cc
        PUBLIC FILE_SET HEADERS
        BASE_DIRS ${OPENFST_INCLUDE_DIR}
        FILES
            ${OPENFST_INCLUDE_DIR}/fst/extensions/linear/linear-fst-data-builder.h
            ${OPENFST_INCLUDE_DIR}/fst/extensions/linear/linear-fst-data.h
            ${OPENFST_INCLUDE_DIR}/fst/extensions/linear/linear-fst.h
            ${OPENFST_INCLUDE_DIR}/fst/extensions/linear/loglinear-apply.h
            ${OPENFST_INCLUDE_DIR}/fst/extensions/linear/trie.h
    )

    target_link_options(${LINEAR_LIBRARY}-fst
        PRIVATE
            LINKER:-avoid-version
            LINKER:-module
    )

    target_link_libraries(${LINEAR_LIBRARY}-fst
        PUBLIC
            fst
    )

    install(
        TARGETS ${LINEAR_LIBRARY}-fst
        EXPORT openfst
        LIBRARY DESTINATION ${CMAKE_INSTALL_LIBDIR}
        ARCHIVE DESTINATION ${CMAKE_INSTALL_LIBDIR}
        RUNTIME DESTINATION ${CMAKE_INSTALL_BINDIR}
        INCLUDES DESTINATION ${CMAKE_INSTALL_INCLUDEDIR}
        FILE_SET HEADERS
    )

    target_include_directories(${LINEAR_LIBRARY}-fst
        PUBLIC
            $<BUILD_INTERFACE:${OPENFST_INCLUDE_DIR}>
            $<INSTALL_INTERFACE:include>
    )
endforeach ()

if (OPENFST_BIN)
    add_library(fstlinearscript)

    set_target_properties(fstlinearscript
        PROPERTIES
            VERSION 23.0.0
            SOVERSION 23
    )

    target_include_directories(fstlinearscript
        PUBLIC
            $<BUILD_INTERFACE:${OPENFST_INCLUDE_DIR}>
            $<INSTALL_INTERFACE:include>
    )

    target_sources(fstlinearscript
        PRIVATE
            linearscript.cc
        PUBLIC FILE_SET HEADERS
        BASE_DIRS ${OPENFST_INCLUDE_DIR}
        FILES
            ${OPENFST_INCLUDE_DIR}/fst/extensions/linear/linearscript.h
    )

    target_link_libraries(fstlinearscript
        PRIVATE
            fstscript
    )

    install(
        TARGETS fstlinearscript
        EXPORT openfst
        LIBRARY DESTINATION ${CMAKE_INSTALL_LIBDIR}
        ARCHIVE DESTINATION ${CMAKE_INSTALL_LIBDIR}
        RUNTIME DESTINATION ${CMAKE_INSTALL_BINDIR}
        INCLUDES DESTINATION ${CMAKE_INSTALL_INCLUDEDIR}
        FILE_SET HEADERS
    )

    foreach (LINEAR_PROGRAM IN LISTS OPENFST_LINEAR_PROGRAMS)
        add_executable(${LINEAR_PROGRAM})

        target_sources(${LINEAR_PROGRAM}
            PRIVATE
                ${LINEAR_PROGRAM}.cc
                ${LINEAR_PROGRAM}-main.cc
        )

        target_link_libraries(${LINEAR_PROGRAM}
            PRIVATE
                fstlinearscript
        )

        install(
            TARGETS ${LINEAR_PROGRAM}
            RUNTIME DESTINATION ${CMAKE_INSTALL_BINDIR}
        )
    endforeach ()
endif ()