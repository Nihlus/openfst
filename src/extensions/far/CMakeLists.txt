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
    PUBLIC FILE_SET HEADERS
    BASE_DIRS ${OPENFST_INCLUDE_DIR}
    FILES
        ${OPENFST_INCLUDE_DIR}/fst/extensions/far/stlist.h
        ${OPENFST_INCLUDE_DIR}/fst/extensions/far/sttable.h
        ${OPENFST_INCLUDE_DIR}/fst/extensions/far/far.h
        ${OPENFST_INCLUDE_DIR}/fst/extensions/far/compile-strings.h
        ${OPENFST_INCLUDE_DIR}/fst/extensions/far/convert.h
        ${OPENFST_INCLUDE_DIR}/fst/extensions/far/create.h
        ${OPENFST_INCLUDE_DIR}/fst/extensions/far/equal.h
        ${OPENFST_INCLUDE_DIR}/fst/extensions/far/extract.h
        ${OPENFST_INCLUDE_DIR}/fst/extensions/far/farlib.h
        ${OPENFST_INCLUDE_DIR}/fst/extensions/far/info.h
        ${OPENFST_INCLUDE_DIR}/fst/extensions/far/isomorphic.h
        ${OPENFST_INCLUDE_DIR}/fst/extensions/far/print-strings.h
)

target_link_libraries(fstfar
    PUBLIC
        fst
)

install(
    TARGETS fstfar
    EXPORT fstfar
    LIBRARY DESTINATION ${CMAKE_INSTALL_LIBDIR}
    ARCHIVE DESTINATION ${CMAKE_INSTALL_LIBDIR}
    RUNTIME DESTINATION ${CMAKE_INSTALL_BINDIR}
    INCLUDES DESTINATION ${CMAKE_INSTALL_INCLUDEDIR}
    FILE_SET HEADERS
)

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
        PUBLIC FILE_SET HEADERS
        BASE_DIRS ${OPENFST_INCLUDE_DIR}
        FILES
            ${OPENFST_INCLUDE_DIR}/fst/extensions/far/compile-strings.h
            ${OPENFST_INCLUDE_DIR}/fst/extensions/far/far-class.h
            ${OPENFST_INCLUDE_DIR}/fst/extensions/far/farscript.h
            ${OPENFST_INCLUDE_DIR}/fst/extensions/far/getters.h
            ${OPENFST_INCLUDE_DIR}/fst/extensions/far/script-impl.h
    )

    target_link_libraries(fstfarscript
        fstfar
        fstscript
    )

    install(
        TARGETS fstfarscript
        EXPORT fstfarscript
        LIBRARY DESTINATION ${CMAKE_INSTALL_LIBDIR}
        ARCHIVE DESTINATION ${CMAKE_INSTALL_LIBDIR}
        RUNTIME DESTINATION ${CMAKE_INSTALL_BINDIR}
        INCLUDES DESTINATION ${CMAKE_INSTALL_INCLUDEDIR}
        FILE_SET HEADERS
    )

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

        install(
            TARGETS ${FAR_PROGRAM}
            RUNTIME DESTINATION ${CMAKE_INSTALL_BINDIR}
        )
    endforeach ()
endif ()