if (OPENFST_COMPACT_FSTS)
    add_subdirectory(compact)
endif ()

if (OPENFST_COMPRESS AND OPENFST_BIN)
    add_subdirectory(compress)
endif ()

if (OPENFST_CONST_FSTS)
    add_subdirectory(const)
endif ()

if (OPENFST_FAR)
    add_subdirectory(far)
endif ()

if (OPENFST_LINEAR_FSTS)
    add_subdirectory(linear)
endif ()

if (OPENFST_LOOKAHEAD_FSTS)
    add_subdirectory(lookahead)
endif ()

if (OPENFST_MPDT AND OPENFST_BIN)
    add_subdirectory(mpdt)
endif ()

if (OPENFST_NGRAM_FSTS)
    add_subdirectory(ngram)
endif ()

if (OPENFST_PYTHON)
    message(WARNING "The Python bindings have not been updated for modern Python environments and may not function or compile correctly")
    add_subdirectory(python)
endif ()

if (OPENFST_PDT AND OPENFST_BIN)
    add_subdirectory(pdt)
endif ()

if (OPENFST_SPECIAL)
    add_subdirectory(special)
endif ()