Project (Id3LibSrc)

Include (${CMAKE_MODULE_PATH}/Utils.cmake)
Set_Target (id3)

Source_Group ("Id3 Headers" FILES ${id3_h_files})
Source_Group ("Generated Files" ${config_h_file})

Create_Shared_Library (
  SOURCE_GLOB "*.cpp;*.h"
  SOURCES ${id3_h_files} ${config_h_file}
  DEPENDENCIES ${ZLIB_LIBRARY}
  )

#Target_Link_Libraries (
#	debug ${LIBCWD_LIBRARY}
#	)
