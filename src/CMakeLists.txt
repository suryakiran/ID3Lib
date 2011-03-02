Project (Id3LibSrc)

File (GLOB src_files *.cpp)

If (CMAKE_COMPILER_IS_GNUCXX)
	Set (
		CMAKE_CXX_FLAGS 
		"${CMAKE_CXX_FLAGS} -Wall -Wno-unused -Wno-inline -Woverloaded-virtual -Wmissing-declarations -Wno-write-strings"
		)
EndIf (CMAKE_COMPILER_IS_GNUCXX)

Add_Library (id3 SHARED ${src_files})

Target_Link_Libraries (
	id3 ${ZLIB_LIBRARIES}
	)
