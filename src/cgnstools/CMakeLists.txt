#############
# cgnstools #
#############

set(Missing "")
add_subdirectory(common)
add_subdirectory(utilities)

file(WRITE ${CMAKE_CURRENT_BINARY_DIR}/cgconfig
"CG_BIN_DIR=${CMAKE_INSTALL_PREFIX}/bin; export CG_BIN_DIR
CG_LIB_DIR=${CMAKE_INSTALL_PREFIX}/share/cgnstools; export CG_LIB_DIR
")

install(PROGRAMS
${CMAKE_CURRENT_BINARY_DIR}/cgconfig
DESTINATION bin)

