cmake_minimum_required(VERSION 3.22)

option(OPENFST_COMPACT_FSTS "Enable compactFst extensions" OFF)
option(OPENFST_COMPRESS "Enable compression extensions" OFF)
option(OPENFST_CONST_FSTS "Enable ConstFst extensions" OFF)
option(OPENFST_FAR "Enable FAR extensions" OFF)
option(OPENFST_LINEAR_FSTS "Enable LinearTagger/ClassifierFst extensions" OFF)
option(OPENFST_LOOKAHEAD_FSTS "Enable LookAheadFst extensions" OFF)
option(OPENFST_MPDT "Enable MPDT extensions" OFF)
option(OPENFST_NGRAM_FSTS "Enable NGramFst extension" OFF)
option(OPENFST_PDT "Enable PDT extensions" OFF)
option(OPENFST_PYTHON "Enable Python extensions" OFF)
option(OPENFST_SPECIAL "Enable special-matcher extensions" OFF)
option(OPENFST_BIN "Enable fst::script and command-line binaries" ON)
option(OPENFST_FSTS "Enable all FST S0s" OFF)
option(OPENFST_GRM "Enable all dependencies of OpenGrm" OFF)

if(OPENFST_FSTS)
    set(OPENFST_COMPACT_FSTS ON)
    set(OPENFST_CONST_FSTS ON)
    set(OPENFST_LINEAR_FSTS ON)
    set(OPENFST_LOOKAHEAD_FSTS ON)
    set(OPENFST_NGRAM_FSTS ON)
endif ()

if (OPENFST_GRM)
    set(OPENFST_FAR ON)
    set(OPENFST_PDT ON)
    set(OPENFST_MPDT ON)
endif ()

if (OPENFST_MPDT)
    set(OPENFST_PDT ON)
endif ()

if (OPENFST_PYTHON)
    set(OPENFST_FAR ON)
endif ()

option(BUILD_SHARED_LIBS "Build shared libraries" OFF)

project(openfst
    LANGUAGES CXX
    VERSION 1.8.0
)

include(GNUInstallDirs)

add_compile_options(
    -fno-exceptions
    -Wno-deprecated-declarations
)

if ("${CMAKE_GENERATOR}" STREQUAL "Xcode")
    # Xcode doesn't like object libraries
    set(OPENFST_OBJECT_LIBRARY_TYPE STATIC)
else ()
    set(OPENFST_OBJECT_LIBRARY_TYPE ${OPENFST_OBJECT_LIBRARY_TYPE})
endif ()

add_subdirectory(src)

install(EXPORT openfst DESTINATION ${CMAKE_INSTALL_PREFIX})
