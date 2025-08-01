# Caution! Be careful, when increase minimal cmake version:
# using of newer version may leads (by default) to applying
# of some new policies. It may break compilation.
# For canceling of applying new policies use:
# cmake_policy(PUSH) before `cmake_minimum_required`
# and cmake_policy(POP) after.
cmake_minimum_required (VERSION 3.1 FATAL_ERROR)

project (ImportExport)

add_definitions (-DWINVER=0x0501 -DUNICODE -D_UNICODE)
set (CMAKE_MFC_FLAG 2)

set (ImportExport_SRC_DIR      ${MFC_STANDARD_SAMPLES_DIR}/03_ImportExport/src)
set (ImportExport_HEADER_FILES ${ImportExport_SRC_DIR}/ColoredShapes.h
                               ${ImportExport_SRC_DIR}/ImportExportApp.h
                               ${ImportExport_SRC_DIR}/ImportExportDoc.h
                               ${ImportExport_SRC_DIR}/StdAfx.h )
set (ImportExport_SOURCE_FILES ${ImportExport_SRC_DIR}/ColoredShapes.cpp
                               ${ImportExport_SRC_DIR}/ImportExportApp.cpp
                               ${ImportExport_SRC_DIR}/ImportExportDoc.cpp
                               ${ImportExport_SRC_DIR}/StdAfx.cpp)

set (ImportExport_RESOURCE_DIR    ${MFC_STANDARD_SAMPLES_DIR}/03_ImportExport/res)
set (ImportExport_RESOURCE_HEADER ${ImportExport_RESOURCE_DIR}/resource.h)
set (ImportExport_RESOURCE_FILES  ${ImportExport_RESOURCE_DIR}/Toolbar.bmp
                                  ${ImportExport_RESOURCE_DIR}/ImportExport.rc)

# groups in the VS solution
source_group ("Source Files" FILES ${ImportExport_SOURCE_FILES}
                                   ${COMMON_WINMAIN_FILE})

source_group ("Header Files" FILES ${ImportExport_HEADER_FILES}
                                   ${ImportExport_RESOURCE_HEADER})

source_group ("Resource Files" FILES ${ImportExport_RESOURCE_FILES})

add_executable (ImportExport WIN32 ${ImportExport_SOURCE_FILES}
                                   ${ImportExport_HEADER_FILES}
                                   ${COMMON_WINMAIN_FILE}
                                   ${ImportExport_RESOURCE_HEADER}
                                   ${ImportExport_RESOURCE_FILES})

set_property (TARGET ImportExport PROPERTY FOLDER "Samples/mfc")

if (SINGLE_GENERATOR)
  install (TARGETS ImportExport DESTINATION "${INSTALL_DIR_BIN}")
else()
  install (TARGETS ImportExport
           CONFIGURATIONS Release RelWithDebInfo
           DESTINATION "${INSTALL_DIR_BIN}")
  install (TARGETS ImportExport
           CONFIGURATIONS Debug
           DESTINATION "${INSTALL_DIR_BIN}d")
endif()

include_directories (${CMAKE_BINARY_DIR}/inc
                     ${MFC_STANDARD_SAMPLES_DIR}/03_ImportExport
                     ${ImportExport_SRC_DIR}
                     ${MFC_STANDARD_SAMPLES_DIR}/Common)

target_link_libraries (ImportExport mfcsample)

set (CMAKE_CXX_STANDARD 17)
set (CMAKE_CXX_STANDARD_REQUIRED ON)