# Copyright (c) 2016, EPFL/Blue Brain Project
#                     Juan Hernando Vieites <juan.hernando@epfl.ch>
#
# This file is part of Brion <https://github.com/BlueBrain/Brion>
#

if(NOT Sphinx_FOUND OR NOT TARGET brion_python)
    message(STATUS "Skipping Python documentation generation")
    return()
endif()

configure_file(conf.py.in ${PROJECT_BINARY_DIR}/doc/python/conf.py)

file(COPY index.rst python_tutorial.rst
  DESTINATION ${PROJECT_BINARY_DIR}/doc/python)

add_custom_target(Brion-python-doc-html
    ${SPHINX_EXECUTABLE} -b html . ${PROJECT_BINARY_DIR}/doc/html/python
    WORKING_DIRECTORY ${PROJECT_BINARY_DIR}/doc/python
    DEPENDS index.rst python_tutorial.rst conf.py.in
    COMMENT "Generating Python documentation using Sphinx" VERBATIM)
set_target_properties(Brion-python-doc-html PROPERTIES
    EXCLUDE_FROM_DEFAULT_BUILD ON FOLDER "doxygen")

    add_dependencies(doxygen Brion-python-doc-html)

if(TARGET brion_python)
    add_dependencies(Brion-python-doc-html brion_python)
endif()
