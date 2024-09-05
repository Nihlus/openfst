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