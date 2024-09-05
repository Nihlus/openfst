set(OPENFST_PDT_PROGRAMS
    pdtcompose
    pdtexpand
    pdtinfo
    pdtreplace
    pdtreverse
    pdtshortestpath
)

add_library(fstpdtscript)

set_target_properties(fstpdtscript
    PROPERTIES
        VERSION 23.0.0
        SOVERSION 23
)

target_sources(fstpdtscript
    PRIVATE
        getters.cc
        pdtscript.cc
)

target_link_libraries(fstpdtscript
        PUBLIC
        fstscript
)

target_include_directories(fstpdtscript
    PUBLIC
        $<BUILD_INTERFACE:${OPENFST_INCLUDE_DIR}>
        $<INSTALL_INTERFACE:include>
)

export(TARGETS fstpdtscript APPEND FILE ${OPENFST_BUILD_TARGETS_FILE})

foreach (PDT_PROGRAM IN LISTS OPENFST_PDT_PROGRAMS)
    add_executable(${PDT_PROGRAM})

    target_sources(${PDT_PROGRAM}
        PRIVATE
            ${PDT_PROGRAM}.cc
            ${PDT_PROGRAM}-main.cc
    )

    target_link_libraries(${PDT_PROGRAM}
        PRIVATE
            fstpdtscript
    )
endforeach ()
