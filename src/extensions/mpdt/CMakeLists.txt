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
    PUBLIC FILE_SET HEADERS
    BASE_DIRS ${OPENFST_INCLUDE_DIR}
    FILES
        ${OPENFST_INCLUDE_DIR}/fst/extensions/mpdt/compose.h
        ${OPENFST_INCLUDE_DIR}/fst/extensions/mpdt/expand.h
        ${OPENFST_INCLUDE_DIR}/fst/extensions/mpdt/info.h
        ${OPENFST_INCLUDE_DIR}/fst/extensions/mpdt/mpdt.h
        ${OPENFST_INCLUDE_DIR}/fst/extensions/mpdt/mpdtlib.h
        ${OPENFST_INCLUDE_DIR}/fst/extensions/mpdt/read_write_utils.h
        ${OPENFST_INCLUDE_DIR}/fst/extensions/mpdt/reverse.h
        ${OPENFST_INCLUDE_DIR}/fst/extensions/mpdt/mpdtscript.h
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

install(
    TARGETS fstmpdtscript
    EXPORT fstmpdtscript
    LIBRARY DESTINATION ${CMAKE_INSTALL_LIBDIR}
    ARCHIVE DESTINATION ${CMAKE_INSTALL_LIBDIR}
    RUNTIME DESTINATION ${CMAKE_INSTALL_BINDIR}
    INCLUDES DESTINATION ${CMAKE_INSTALL_INCLUDEDIR}
    FILE_SET HEADERS
)

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

    install(
        TARGETS ${MPDT_PROGRAM}
        RUNTIME DESTINATION ${CMAKE_INSTALL_BINDIR}
    )
endforeach ()
