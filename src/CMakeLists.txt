Project (Id3LibSrc)
Set (target id3)

File (GLOB src_files *.cpp)
File (GLOB h_files *.h)

If (CMAKE_COMPILER_IS_GNUCXX)
	Set (
		CMAKE_CXX_FLAGS 
		"${CMAKE_CXX_FLAGS} -Wall -Wno-unused -Wno-inline -Woverloaded-virtual -Wmissing-declarations -Wno-write-strings"
		)
EndIf (CMAKE_COMPILER_IS_GNUCXX)

Source_Group ("Id3 Headers" FILES ${id3_h_files})
Source_Group ("Generated Files" ${config_h_file})
Add_Library (${target} SHARED ${src_files} ${h_files} ${id3_h_files} ${config_h_file})

Target_Link_Libraries (
	${target} ${ZLIB_LIBRARY}
	debug ${LIBCWD_LIBRARY}
	)

If (WIN32)
Set_Target_Properties (${target} PROPERTIES 
	PREFIX lib
	IMPORT_PREFIX lib
	)
EndIf (WIN32)
