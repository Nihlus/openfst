add_library(fst)

set_target_properties(fst
    PROPERTIES
        VERSION 23.0.0
        SOVERSION 23
)

target_sources(fst
    PRIVATE
        compat.cc
        encode.cc
        flags.cc
        fst.cc
        fst-types.cc
        mapped-file.cc
        properties.cc
        symbol-table.cc
        symbol-table-ops.cc
        weight.cc
        util.cc
)

target_include_directories(fst
    PUBLIC
        $<BUILD_INTERFACE:${OPENFST_INCLUDE_DIR}>
        $<INSTALL_INTERFACE:include>
)

export(TARGETS fst APPEND FILE ${OPENFST_BUILD_TARGETS_FILE})
