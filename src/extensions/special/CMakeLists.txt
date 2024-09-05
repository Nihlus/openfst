set(OPENFST_SPECIAL_LIBRARIES
    phi
    rho
    sigma
)

add_library(fstspecial)
set_target_properties(fstspecial
    PROPERTIES
        VERSION 23.0.0
        SOVERSION 23
)

target_include_directories(fstspecial
    PUBLIC
        $<BUILD_INTERFACE:${OPENFST_INCLUDE_DIR}>
        $<INSTALL_INTERFACE:include>
)

export(TARGETS fstspecial APPEND FILE ${OPENFST_BUILD_TARGETS_FILE})

foreach (SPECIAL_LIBRARY IN LISTS OPENFST_SPECIAL_LIBRARIES)
    add_library(${SPECIAL_LIBRARY}-fst)
    set_target_properties(${SPECIAL_LIBRARY}-fst
        PROPERTIES
            VERSION 23.0.0
            SOVERSION 23
    )

    target_include_directories(${SPECIAL_LIBRARY}-fst
        PUBLIC
            $<BUILD_INTERFACE:${OPENFST_INCLUDE_DIR}>
            $<INSTALL_INTERFACE:include>
    )

    target_sources(${SPECIAL_LIBRARY}-fst
        PRIVATE
            ${SPECIAL_LIBRARY}-fst.cc
    )

    target_link_options(${SPECIAL_LIBRARY}-fst
        PRIVATE
            LINKER:-avoid-version
            LINKER:-module
    )

    target_link_libraries(${SPECIAL_LIBRARY}-fst
        PUBLIC
            fst
    )

    target_sources(fstspecial
        PRIVATE
            $<TARGET_OBJECTS:${SPECIAL_LIBRARY}-fst>
    )
endforeach ()

if (OPENFST_BIN)
    add_executable(fstspecial-bin)

    set_target_properties(fstspecial-bin
        PROPERTIES
            OUTPUT_NAME fstspecial
    )

    target_sources(fstspecial-bin
        PRIVATE
            fstspecial.cc
    )

    target_link_libraries(fstspecial-bin
        PRIVATE
            fstspecial
            fstscript
    )

    target_include_directories(fstspecial-bin
        PRIVATE
            ${OPENFST_BIN_SOURCE_DIR}
    )
endif ()
