#############
# utilities #
#############

include_directories(../..)
include_directories(../calclib)
include_directories(../common)

# cgns_info
if (CGNS_USE_SHARED)
  link_libraries(cgns_shared)
else (CGNS_USE_SHARED)
  link_libraries(cgns_static)
endif (CGNS_USE_SHARED)
if (NOT WIN32)
  link_libraries(m)
endif (NOT WIN32)

if (CGNS_ENABLE_HDF5 AND HDF5_LIBRARY)
  link_libraries(${HDF5_LIBRARY})
  if(HDF5_NEED_ZLIB AND ZLIB_LIBRARY)
    link_libraries(${ZLIB_LIBRARY})
  endif(HDF5_NEED_ZLIB AND ZLIB_LIBRARY)
  if(HDF5_NEED_SZIP AND SZIP_LIBRARY)
    link_libraries(${SZIP_LIBRARY})
  endif(HDF5_NEED_SZIP AND SZIP_LIBRARY)
  if(HDF5_NEED_MPI AND MPI_LIBS)
    link_libraries(${MPI_LIBS})
  endif(HDF5_NEED_MPI AND MPI_LIBS)
endif (CGNS_ENABLE_HDF5 AND HDF5_LIBRARY)

if (NOT WIN32)
  link_libraries(m)
endif (NOT WIN32)

set(cgns_info_FILES
	cgns_info.c
	cgnsutil.c
	../common/getargs.c)

add_executable(cgns_info ${cgns_info_FILES})
add_dependencies(cgns_info cgns_shared)

# aflr3_to_cgns
set(aflr3_to_cgns_FILES
	aflr3_to_cgns.c
	binaryio.c
	../common/getargs.c)

add_executable(aflr3_to_cgns ${aflr3_to_cgns_FILES})
add_dependencies(aflr3_to_cgns cgns_shared)

# cgns_to_aflr3
set(cgns_to_aflr3_FILES
	cgns_to_aflr3.c
	../common/getargs.c
	../common/hash.c)

add_executable(cgns_to_aflr3 ${cgns_to_aflr3_FILES})
add_dependencies(cgns_to_aflr3 cgns_shared)

# cgns_to_vtk
set(cgns_to_vtk_FILES
	cgns_to_vtk.c
	../common/getargs.c)

add_executable(cgns_to_vtk ${cgns_to_vtk_FILES})
add_dependencies(cgns_to_vtk cgns_shared)

install(TARGETS
cgns_info
aflr3_to_cgns
cgns_to_aflr3
cgns_to_vtk
RUNTIME DESTINATION bin/cgnstools)

install(FILES
conserved.cnv
convert.tcl
dimensional.cnv
patran.tcl
plot3d.tcl
primitive.cnv
tecplot.tcl
tetgen.tcl
vgrid.tcl
aflr3.tcl
fast.tcl
util.tcl
utilities.mnu
vtk.tcl
DESTINATION share/cgnstools)

