set(OPENFST_TEST_PROGRAMS
    fst
    weight
    algo
)

set(OPENFST_ALGO_TEST_PROGRAMS
    log
    tropical
    minmax
    lexicographic
    power
)

foreach (TEST_PROGRAM IN LISTS OPENFST_TEST_PROGRAMS)
    if ("${TEST_PROGRAM}" STREQUAL "algo")
        foreach (ALGO_TEST_PROGRAM IN LISTS OPENFST_ALGO_TEST_PROGRAMS)

            add_executable(${TEST_PROGRAM}_test_${ALGO_TEST_PROGRAM})

            target_sources(${TEST_PROGRAM}_test_${ALGO_TEST_PROGRAM}
                PRIVATE
                    ${TEST_PROGRAM}_test.cc
            )

            string(TOUPPER ALGO_TEST_DEFINE "${ALGO_TEST_PROGRAM}")
            target_compile_definitions(${TEST_PROGRAM}_test_${ALGO_TEST_PROGRAM}
                PRIVATE
                    TEST_${ALGO_TEST_DEFINE}
            )

            target_link_libraries(${TEST_PROGRAM}_test_${ALGO_TEST_PROGRAM}
                PRIVATE
                    fst
            )

            add_test(
                NAME ${TEST_PROGRAM}_${ALGO_TEST_PROGRAM}
                COMMAND ${TEST_PROGRAM}_test_${ALGO_TEST_PROGRAM}
            )
        endforeach ()
    else ()
        add_executable(${TEST_PROGRAM}_test)

        target_sources(${TEST_PROGRAM}_test
            PRIVATE
                ${TEST_PROGRAM}_test.cc
        )

        target_link_libraries(${TEST_PROGRAM}_test
            PRIVATE
                fst
        )

        add_test(
            NAME ${TEST_PROGRAM}
            COMMAND ${TEST_PROGRAM}_test
        )
    endif ()
endforeach ()