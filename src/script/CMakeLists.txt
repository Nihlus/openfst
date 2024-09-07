add_library(fstscript)

set_target_properties(fstscript
    PROPERTIES
        VERSION 23.0.0
        SOVERSION 23
)

target_sources(fstscript
    PRIVATE
        arciterator-class.cc
        arcsort.cc
        closure.cc
        compile.cc
        compose.cc
        concat.cc
        connect.cc
        convert.cc
        decode.cc
        determinize.cc
        difference.cc
        disambiguate.cc
        draw.cc
        encode.cc
        encodemapper-class.cc
        epsnormalize.cc
        equal.cc
        equivalent.cc
        fst-class.cc
        getters.cc
        info-impl.cc
        info.cc
        intersect.cc
        invert.cc
        isomorphic.cc map.cc
        minimize.cc
        print.cc
        project.cc
        prune.cc
        push.cc
        randequivalent.cc
        randgen.cc
        relabel.cc
        replace.cc
        reverse.cc
        reweight.cc
        rmepsilon.cc
        shortest-distance.cc
        shortest-path.cc
        stateiterator-class.cc
        synchronize.cc
        text-io.cc
        topsort.cc
        union.cc
        weight-class.cc
        verify.cc
    PUBLIC FILE_SET HEADERS
    BASE_DIRS ${OPENFST_INCLUDE_DIR}
    FILES
        ${OPENFST_INCLUDE_DIR}/fst/script/arc-class.h
        ${OPENFST_INCLUDE_DIR}/fst/script/arciterator-class.h
        ${OPENFST_INCLUDE_DIR}/fst/script/arg-packs.h
        ${OPENFST_INCLUDE_DIR}/fst/script/encodemapper-class.h
        ${OPENFST_INCLUDE_DIR}/fst/script/fst-class.h
        ${OPENFST_INCLUDE_DIR}/fst/script/fstscript-decl.h
        ${OPENFST_INCLUDE_DIR}/fst/script/fstscript.h
        ${OPENFST_INCLUDE_DIR}/fst/script/getters.h
        ${OPENFST_INCLUDE_DIR}/fst/script/script-impl.h
        ${OPENFST_INCLUDE_DIR}/fst/script/stateiterator-class.h
        ${OPENFST_INCLUDE_DIR}/fst/script/text-io.h
        ${OPENFST_INCLUDE_DIR}/fst/script/weight-class.h
        ${OPENFST_INCLUDE_DIR}/fst/script/arcsort.h
        ${OPENFST_INCLUDE_DIR}/fst/script/closure.h
        ${OPENFST_INCLUDE_DIR}/fst/script/compile.h
        ${OPENFST_INCLUDE_DIR}/fst/script/compile-impl.h
        ${OPENFST_INCLUDE_DIR}/fst/script/compose.h
        ${OPENFST_INCLUDE_DIR}/fst/script/concat.h
        ${OPENFST_INCLUDE_DIR}/fst/script/connect.h
        ${OPENFST_INCLUDE_DIR}/fst/script/convert.h
        ${OPENFST_INCLUDE_DIR}/fst/script/decode.h
        ${OPENFST_INCLUDE_DIR}/fst/script/determinize.h
        ${OPENFST_INCLUDE_DIR}/fst/script/difference.h
        ${OPENFST_INCLUDE_DIR}/fst/script/disambiguate.h
        ${OPENFST_INCLUDE_DIR}/fst/script/draw.h
        ${OPENFST_INCLUDE_DIR}/fst/script/draw-impl.h
        ${OPENFST_INCLUDE_DIR}/fst/script/encode.h
        ${OPENFST_INCLUDE_DIR}/fst/script/epsnormalize.h
        ${OPENFST_INCLUDE_DIR}/fst/script/equal.h
        ${OPENFST_INCLUDE_DIR}/fst/script/equivalent.h
        ${OPENFST_INCLUDE_DIR}/fst/script/info.h
        ${OPENFST_INCLUDE_DIR}/fst/script/info-impl.h
        ${OPENFST_INCLUDE_DIR}/fst/script/intersect.h
        ${OPENFST_INCLUDE_DIR}/fst/script/invert.h
        ${OPENFST_INCLUDE_DIR}/fst/script/isomorphic.h
        ${OPENFST_INCLUDE_DIR}/fst/script/map.h
        ${OPENFST_INCLUDE_DIR}/fst/script/minimize.h
        ${OPENFST_INCLUDE_DIR}/fst/script/print.h
        ${OPENFST_INCLUDE_DIR}/fst/script/print-impl.h
        ${OPENFST_INCLUDE_DIR}/fst/script/project.h
        ${OPENFST_INCLUDE_DIR}/fst/script/prune.h
        ${OPENFST_INCLUDE_DIR}/fst/script/push.h
        ${OPENFST_INCLUDE_DIR}/fst/script/randequivalent.h
        ${OPENFST_INCLUDE_DIR}/fst/script/randgen.h
        ${OPENFST_INCLUDE_DIR}/fst/script/relabel.h
        ${OPENFST_INCLUDE_DIR}/fst/script/replace.h
        ${OPENFST_INCLUDE_DIR}/fst/script/reverse.h
        ${OPENFST_INCLUDE_DIR}/fst/script/reweight.h
        ${OPENFST_INCLUDE_DIR}/fst/script/rmepsilon.h
        ${OPENFST_INCLUDE_DIR}/fst/script/shortest-distance.h
        ${OPENFST_INCLUDE_DIR}/fst/script/shortest-path.h
        ${OPENFST_INCLUDE_DIR}/fst/script/synchronize.h
        ${OPENFST_INCLUDE_DIR}/fst/script/topsort.h
        ${OPENFST_INCLUDE_DIR}/fst/script/union.h
        ${OPENFST_INCLUDE_DIR}/fst/script/verify.h
)

target_include_directories(fstscript
    PUBLIC
        $<BUILD_INTERFACE:${OPENFST_INCLUDE_DIR}>
        $<INSTALL_INTERFACE:include>
)

target_link_libraries(fstscript
    PUBLIC
        fst
)

install(
    TARGETS fstscript
    EXPORT openfst
    LIBRARY DESTINATION ${CMAKE_INSTALL_LIBDIR}
    ARCHIVE DESTINATION ${CMAKE_INSTALL_LIBDIR}
    RUNTIME DESTINATION ${CMAKE_INSTALL_BINDIR}
    INCLUDES DESTINATION ${CMAKE_INSTALL_INCLUDEDIR}
    FILE_SET HEADERS
)
