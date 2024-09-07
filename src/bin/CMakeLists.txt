set(OPENFST_PROGRAMS
    fstarcsort
    fstclosure
    fstcompile
    fstcompose
    fstconcat
    fstconnect
    fstconvert
    fstdeterminize
    fstdifference
    fstdisambiguate
    fstdraw
    fstencode
    fstepsnormalize
    fstequal
    fstequivalent
    fstinfo
    fstintersect
    fstinvert
    fstisomorphic
    fstmap
    fstminimize
    fstprint
    fstproject
    fstprune
    fstpush
    fstrandgen
    fstrelabel
    fstreplace
    fstreverse
    fstreweight
    fstrmepsilon
    fstshortestdistance
    fstshortestpath
    fstsymbols
    fstsynchronize
    fsttopsort
    fstunion
)

foreach (PROGRAM IN LISTS OPENFST_PROGRAMS)
    add_executable(${PROGRAM})

    target_compile_features(${PROGRAM}
        PUBLIC
            cxx_std_17
    )

    target_sources(${PROGRAM}
        PRIVATE
            ${PROGRAM}.cc
            ${PROGRAM}-main.cc
    )

    target_link_libraries(${PROGRAM}
        PRIVATE
            fstscript
    )

    install(
        TARGETS ${PROGRAM}
        RUNTIME DESTINATION ${CMAKE_INSTALL_BINDIR}
    )
endforeach ()
