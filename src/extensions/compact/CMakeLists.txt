set(OPENFST_COMPACT_LIBRARIES
    compact8_acceptor
    compact8_string
    compact8_unweighted
    compact8_unweighted_acceptor
    compact8_weighted_string
    compact16_acceptor
    compact16_string
    compact16_unweighted
    compact16_unweighted_acceptor
    compact16_weighted_string
    compact64_acceptor
    compact64_string
    compact64_unweighted
    compact64_unweighted_acceptor
    compact64_weighted_string
)

add_library(fstcompact)
set_target_properties(fstcompact
    PROPERTIES
        VERSION 23.0.0
        SOVERSION 23
)

target_include_directories(fstcompact
    PUBLIC
        $<BUILD_INTERFACE:${OPENFST_INCLUDE_DIR}>
        $<INSTALL_INTERFACE:include>
)

export(TARGETS fstcompact APPEND FILE ${OPENFST_BUILD_TARGETS_FILE})

foreach (COMPACT_LIBRARY IN LISTS OPENFST_COMPACT_LIBRARIES)
    add_library(${COMPACT_LIBRARY}-fst)
    set_target_properties(${COMPACT_LIBRARY}-fst
        PROPERTIES
            VERSION 23.0.0
            SOVERSION 23
    )

    target_include_directories(${COMPACT_LIBRARY}-fst
        PUBLIC
            $<BUILD_INTERFACE:${OPENFST_INCLUDE_DIR}>
            $<INSTALL_INTERFACE:include>
    )

    target_sources(${COMPACT_LIBRARY}-fst
        PRIVATE
            ${COMPACT_LIBRARY}-fst.cc
    )

    target_link_options(${COMPACT_LIBRARY}-fst
        PRIVATE
            LINKER:-avoid-version
            LINKER:-module
    )

    target_link_libraries(${COMPACT_LIBRARY}-fst
        PUBLIC
            fst
    )

    target_sources(fstcompact
        PRIVATE
            $<TARGET_OBJECTS:${COMPACT_LIBRARY}-fst>
    )
endforeach ()
