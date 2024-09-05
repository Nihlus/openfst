set(OPENFST_FAR_PROGRAMS
    farcompilestrings
    farconvert
    farcreate
    farequal
    farextract
    farinfo
    farisomorphic
    farprintstrings
)

add_library(fstfar)

set_target_properties(fstfar
    PROPERTIES
        VERSION 23.0.0
        SOVERSION 23
)

target_include_directories(fstfar
    PUBLIC
        $<BUILD_INTERFACE:${OPENFST_INCLUDE_DIR}>
        $<INSTALL_INTERFACE:include>
)

target_sources(fstfar
    PRIVATE
        sttable.cc
        stlist.cc
)

target_link_libraries(fstfar
    PUBLIC
        fst
)

export(TARGETS fstfar APPEND FILE ${OPENFST_BUILD_TARGETS_FILE})

if (OPENFST_BIN)
    add_library(fstfarscript)

    set_target_properties(fstfarscript
        PROPERTIES
            VERSION 23.0.0
            SOVERSION 23
    )

    target_include_directories(fstfarscript
        PUBLIC
            $<BUILD_INTERFACE:${OPENFST_INCLUDE_DIR}>
            $<INSTALL_INTERFACE:include>
    )

    target_sources(fstfarscript
        PRIVATE
            far-class.cc
            farscript.cc
            getters.cc
            script-impl.cc
            strings.cc
    )

    target_link_libraries(fstfarscript
        fstfar
        fstscript
    )

    export(TARGETS fstfarscript APPEND FILE ${OPENFST_BUILD_TARGETS_FILE})

    foreach (FAR_PROGRAM IN LISTS OPENFST_FAR_PROGRAMS)
        add_executable(${FAR_PROGRAM})

        target_sources(${FAR_PROGRAM}
            PRIVATE
                ${FAR_PROGRAM}.cc
                ${FAR_PROGRAM}-main.cc
        )

        target_link_libraries(${FAR_PROGRAM}
            PRIVATE
                fstfarscript
        )
    endforeach ()
endif ()