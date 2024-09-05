add_library(pywrapfst)

find_package(Python3 REQUIRED
    COMPONENTS
        Development
)

set_target_properties(pywrapfst
    PROPERTIES
        VERSION 23.0.0
        SOVERSION 23
)

target_sources(pywrapfst
    PRIVATE
        pywrapfst.cpp
)

target_compile_options(pywrapfst
    PRIVATE
        -fexceptions
)

target_link_options(pywrapfst
    PRIVATE
        LINKER:-avoid-version
        LINKER:-module
)

target_link_libraries(pywrapfst
    PRIVATE
        Python3::Module
        fstfarscript
)
