set(OPENFST_CONST_LIBRARIES
    const8
    const16
    const64
)

add_library(fstconst)
set_target_properties(fstconst
    PROPERTIES
        VERSION 23.0.0
        SOVERSION 23
)

target_include_directories(fstconst
    PUBLIC
        $<BUILD_INTERFACE:${OPENFST_INCLUDE_DIR}>
        $<INSTALL_INTERFACE:include>
)

export(TARGETS fstconst APPEND FILE ${OPENFST_BUILD_TARGETS_FILE})

foreach (CONST_LIBRARY IN LISTS OPENFST_CONST_LIBRARIES)
    add_library(${CONST_LIBRARY}-fst)
    set_target_properties(${CONST_LIBRARY}-fst
        PROPERTIES
            VERSION 23.0.0
            SOVERSION 23
    )

    target_include_directories(${CONST_LIBRARY}-fst
        PUBLIC
            $<BUILD_INTERFACE:${OPENFST_INCLUDE_DIR}>
            $<INSTALL_INTERFACE:include>
    )

    target_sources(${CONST_LIBRARY}-fst
        PRIVATE
            ${CONST_LIBRARY}-fst.cc
    )

    target_link_options(${CONST_LIBRARY}-fst
        PRIVATE
            LINKER:-avoid-version
            LINKER:-module
    )

    target_link_libraries(${CONST_LIBRARY}-fst
        PUBLIC
            fst
    )

    target_sources(fstconst
        PRIVATE
            $<TARGET_OBJECTS:${CONST_LIBRARY}-fst>
    )
endforeach ()