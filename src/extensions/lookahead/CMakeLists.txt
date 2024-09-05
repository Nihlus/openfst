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

target_include_directories(fstlookahead
    PUBLIC
        $<BUILD_INTERFACE:${OPENFST_INCLUDE_DIR}>
        $<INSTALL_INTERFACE:include>
)

export(TARGETS fstlookahead APPEND FILE ${OPENFST_BUILD_TARGETS_FILE})

foreach (LOOKAHEAD_LIBRARY IN LISTS OPENFST_LOOKAHEAD_LIBRARIES)
    add_library(${LOOKAHEAD_LIBRARY}-fst)
    set_target_properties(${LOOKAHEAD_LIBRARY}-fst
        PROPERTIES
            VERSION 23.0.0
            SOVERSION 23
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

    target_sources(fstlookahead
        PRIVATE
            $<TARGET_OBJECTS:${LOOKAHEAD_LIBRARY}-fst>
    )
endforeach ()
