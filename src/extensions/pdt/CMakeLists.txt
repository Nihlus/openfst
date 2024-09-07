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
    PUBLIC FILE_SET HEADERS
    BASE_DIRS ${OPENFST_INCLUDE_DIR}
    FILES
        ${OPENFST_INCLUDE_DIR}/fst/extensions/pdt/collection.h
        ${OPENFST_INCLUDE_DIR}/fst/extensions/pdt/compose.h
        ${OPENFST_INCLUDE_DIR}/fst/extensions/pdt/expand.h
        ${OPENFST_INCLUDE_DIR}/fst/extensions/pdt/getters.h
        ${OPENFST_INCLUDE_DIR}/fst/extensions/pdt/info.h
        ${OPENFST_INCLUDE_DIR}/fst/extensions/pdt/paren.h
        ${OPENFST_INCLUDE_DIR}/fst/extensions/pdt/pdt.h
        ${OPENFST_INCLUDE_DIR}/fst/extensions/pdt/pdtlib.h
        ${OPENFST_INCLUDE_DIR}/fst/extensions/pdt/replace.h
        ${OPENFST_INCLUDE_DIR}/fst/extensions/pdt/reverse.h
        ${OPENFST_INCLUDE_DIR}/fst/extensions/pdt/shortest-path.h
        ${OPENFST_INCLUDE_DIR}/fst/extensions/pdt/pdtscript.h
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

install(
    TARGETS fstpdtscript
    EXPORT openfst
    LIBRARY DESTINATION ${CMAKE_INSTALL_LIBDIR}
    ARCHIVE DESTINATION ${CMAKE_INSTALL_LIBDIR}
    RUNTIME DESTINATION ${CMAKE_INSTALL_BINDIR}
    INCLUDES DESTINATION ${CMAKE_INSTALL_INCLUDEDIR}
    FILE_SET HEADERS
)

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

    install(
        TARGETS ${PDT_PROGRAM}
        RUNTIME DESTINATION ${CMAKE_INSTALL_BINDIR}
    )
endforeach ()
