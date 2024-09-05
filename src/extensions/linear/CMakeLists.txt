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
    )

    target_link_libraries(fstlinearscript
        PRIVATE
            fstscript
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
    endforeach ()

    export(TARGETS fstlinearscript APPEND FILE ${OPENFST_BUILD_TARGETS_FILE})
endif ()