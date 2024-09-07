add_library(fst)

set_target_properties(fst
    PROPERTIES
        VERSION 23.0.0
        SOVERSION 23
)

target_compile_features(fst
    PUBLIC
        cxx_std_17
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
    PUBLIC
        FILE_SET HEADERS
        BASE_DIRS ${OPENFST_INCLUDE_DIR}
        FILES
        ${OPENFST_INCLUDE_DIR}/fst/fstlib.h
        ${OPENFST_INCLUDE_DIR}/fst/accumulator.h
        ${OPENFST_INCLUDE_DIR}/fst/add-on.h
        ${OPENFST_INCLUDE_DIR}/fst/arc-arena.h
        ${OPENFST_INCLUDE_DIR}/fst/arc-map.h
        ${OPENFST_INCLUDE_DIR}/fst/arc.h
        ${OPENFST_INCLUDE_DIR}/fst/arcfilter.h
        ${OPENFST_INCLUDE_DIR}/fst/arcsort.h
        ${OPENFST_INCLUDE_DIR}/fst/bi-table.h
        ${OPENFST_INCLUDE_DIR}/fst/cache.h
        ${OPENFST_INCLUDE_DIR}/fst/closure.h
        ${OPENFST_INCLUDE_DIR}/fst/compact-fst.h
        ${OPENFST_INCLUDE_DIR}/fst/complement.h
        ${OPENFST_INCLUDE_DIR}/fst/compose-filter.h
        ${OPENFST_INCLUDE_DIR}/fst/compose.h
        ${OPENFST_INCLUDE_DIR}/fst/concat.h
        ${OPENFST_INCLUDE_DIR}/fst/connect.h
        ${OPENFST_INCLUDE_DIR}/fst/const-fst.h
        ${OPENFST_INCLUDE_DIR}/fst/determinize.h
        ${OPENFST_INCLUDE_DIR}/fst/dfs-visit.h
        ${OPENFST_INCLUDE_DIR}/fst/difference.h
        ${OPENFST_INCLUDE_DIR}/fst/disambiguate.h
        ${OPENFST_INCLUDE_DIR}/fst/edit-fst.h
        ${OPENFST_INCLUDE_DIR}/fst/encode.h
        ${OPENFST_INCLUDE_DIR}/fst/epsnormalize.h
        ${OPENFST_INCLUDE_DIR}/fst/equal.h
        ${OPENFST_INCLUDE_DIR}/fst/equivalent.h
        ${OPENFST_INCLUDE_DIR}/fst/expanded-fst.h
        ${OPENFST_INCLUDE_DIR}/fst/factor-weight.h
        ${OPENFST_INCLUDE_DIR}/fst/filter-state.h
        ${OPENFST_INCLUDE_DIR}/fst/fst.h
        ${OPENFST_INCLUDE_DIR}/fst/heap.h
        ${OPENFST_INCLUDE_DIR}/fst/intersect.h
        ${OPENFST_INCLUDE_DIR}/fst/invert.h
        ${OPENFST_INCLUDE_DIR}/fst/isomorphic.h
        ${OPENFST_INCLUDE_DIR}/fst/label-reachable.h
        ${OPENFST_INCLUDE_DIR}/fst/lookahead-filter.h
        ${OPENFST_INCLUDE_DIR}/fst/lookahead-matcher.h
        ${OPENFST_INCLUDE_DIR}/fst/map.h
        ${OPENFST_INCLUDE_DIR}/fst/matcher-fst.h
        ${OPENFST_INCLUDE_DIR}/fst/matcher.h
        ${OPENFST_INCLUDE_DIR}/fst/memory.h
        ${OPENFST_INCLUDE_DIR}/fst/minimize.h
        ${OPENFST_INCLUDE_DIR}/fst/mutable-fst.h
        ${OPENFST_INCLUDE_DIR}/fst/partition.h
        ${OPENFST_INCLUDE_DIR}/fst/project.h
        ${OPENFST_INCLUDE_DIR}/fst/properties.h
        ${OPENFST_INCLUDE_DIR}/fst/prune.h
        ${OPENFST_INCLUDE_DIR}/fst/push.h
        ${OPENFST_INCLUDE_DIR}/fst/queue.h
        ${OPENFST_INCLUDE_DIR}/fst/randequivalent.h
        ${OPENFST_INCLUDE_DIR}/fst/randgen.h
        ${OPENFST_INCLUDE_DIR}/fst/rational.h
        ${OPENFST_INCLUDE_DIR}/fst/relabel.h
        ${OPENFST_INCLUDE_DIR}/fst/replace-util.h
        ${OPENFST_INCLUDE_DIR}/fst/replace.h
        ${OPENFST_INCLUDE_DIR}/fst/reverse.h
        ${OPENFST_INCLUDE_DIR}/fst/reweight.h
        ${OPENFST_INCLUDE_DIR}/fst/rmepsilon.h
        ${OPENFST_INCLUDE_DIR}/fst/rmfinalepsilon.h
        ${OPENFST_INCLUDE_DIR}/fst/shortest-distance.h
        ${OPENFST_INCLUDE_DIR}/fst/shortest-path.h
        ${OPENFST_INCLUDE_DIR}/fst/state-map.h
        ${OPENFST_INCLUDE_DIR}/fst/state-reachable.h
        ${OPENFST_INCLUDE_DIR}/fst/state-table.h
        ${OPENFST_INCLUDE_DIR}/fst/statesort.h
        ${OPENFST_INCLUDE_DIR}/fst/string.h
        ${OPENFST_INCLUDE_DIR}/fst/symbol-table-ops.h
        ${OPENFST_INCLUDE_DIR}/fst/synchronize.h
        ${OPENFST_INCLUDE_DIR}/fst/test-properties.h
        ${OPENFST_INCLUDE_DIR}/fst/topsort.h
        ${OPENFST_INCLUDE_DIR}/fst/union.h
        ${OPENFST_INCLUDE_DIR}/fst/vector-fst.h
        ${OPENFST_INCLUDE_DIR}/fst/verify.h
        ${OPENFST_INCLUDE_DIR}/fst/visit.h
)

target_include_directories(fst
    PUBLIC
        $<BUILD_INTERFACE:${OPENFST_INCLUDE_DIR}>
        $<INSTALL_INTERFACE:include>
)

install(
    TARGETS fst
    EXPORT openfst
    LIBRARY DESTINATION ${CMAKE_INSTALL_LIBDIR}
    ARCHIVE DESTINATION ${CMAKE_INSTALL_LIBDIR}
    RUNTIME DESTINATION ${CMAKE_INSTALL_BINDIR}
    INCLUDES DESTINATION ${CMAKE_INSTALL_INCLUDEDIR}
    FILE_SET HEADERS
)
