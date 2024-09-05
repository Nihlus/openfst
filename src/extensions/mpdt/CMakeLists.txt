set(OPENFST_MPDT_PROGRAMS
    mpdtcompose
    mpdtexpand
    mpdtinfo
    mpdtreverse
)

add_library(fstmpdtscript)

set_target_properties(fstmpdtscript
    PROPERTIES
        VERSION 23.0.0
        SOVERSION 23
)

target_sources(fstmpdtscript
    PRIVATE
        mpdtscript.cc
)

target_link_libraries(fstmpdtscript
    PUBLIC
        fstpdtscript
)

target_include_directories(fstmpdtscript
    PUBLIC
        $<BUILD_INTERFACE:${OPENFST_INCLUDE_DIR}>
        $<INSTALL_INTERFACE:include>
)

export(TARGETS fstmpdtscript APPEND FILE ${OPENFST_BUILD_TARGETS_FILE})

foreach (MPDT_PROGRAM IN LISTS OPENFST_MPDT_PROGRAMS)
    add_executable(${MPDT_PROGRAM})

    target_sources(${MPDT_PROGRAM}
        PRIVATE
            ${MPDT_PROGRAM}.cc
            ${MPDT_PROGRAM}-main.cc
    )

    target_link_libraries(${MPDT_PROGRAM}
        PRIVATE
            fstmpdtscript
    )
endforeach ()
