Project (ID3Lib)

Set (ID3LIB_MAJOR_VERSION 3)
Set (ID3LIB_MINOR_VERSION 8)
Set (ID3LIB_PATCH_VERSION 3)
Set (ID3LIB_ADDED_VERSION)
Set (ID3LIB_INTERFACE_AGE 0)
Set (ID3LIB_BINARY_AGE 0)
Set (ID3LIB_NAME "id3lib")
Set (
	ID3LIB_VERSION
	${ID3LIB_MAJOR_VERSION}.${ID3LIB_MINOR_VERSION}.${ID3LIB_PATCH_VERSION}
	)

If (ID3LIB_ADDED_VERSION)
	Set (ID3LIB_VERSION ${ID3LIB_VERSION}.${ID3LIB_ADDED_VERSION})
EndIf (ID3LIB_ADDED_VERSION)

If (WIN32)
	Add_Definitions ("-DID3LIB_LINKOPTION=2")
	Add_Definitions ("-D_SCL_SECURE_NO_WARNINGS")
	Add_Definitions ("-D_CRT_SECURE_NO_WARNINGS")
EndIf (WIN32)

Set (ID3LIB_DOC_OUTDIR ${CMAKE_CURRENT_BINARY_DIR}/doxygen)
Set (ID3LIB_SOURCE_DIR ${CMAKE_CURRENT_SOURCE_DIR} PARENT_SCOPE)

Include (CheckIncludeFile)
Include (CheckIncludeFiles)
Include (CheckIncludeFileCXX)
Include (CheckSymbolExists)
Include (${CMAKE_MODULE_PATH}/Find.cmake)

Check_Include_File_Cxx (cctype HAVE_CCTYPE)
Check_Include_File_Cxx (climits HAVE_CLIMITS)
Check_Include_File_Cxx (cstdio HAVE_CSTDIO)
Check_Include_File_Cxx (cstdlib HAVE_CSTDLIB)
Check_Include_File_Cxx (cstring HAVE_CSTRING)
Check_Include_File_Cxx (fstream HAVE_FSTREAM)
Check_Include_File_Cxx (iomanip HAVE_IOMANIP)
Check_Include_File_Cxx (iostream HAVE_IOSTREAM)
Check_Include_File_Cxx (bitset HAVE_BITSET)
Check_Include_File_Cxx (string HAVE_STRING)
Check_Include_File_Cxx (fstream.h HAVE_FSTREAM_H)
Check_Include_File_Cxx (iomanip.h HAVE_IOMANIP_H)
Check_Include_File_Cxx (algorithm HAVE_ALGORITHM)

Check_Include_File (iconv.h HAVE_ICONV_H)
Check_Include_File (dlfcn.h HAVE_DLFCN_H)
Check_Include_File (inttypes.h HAVE_INTTYPES_H)
Check_Include_File (memory.h HAVE_MEMORY_H)
Check_Include_File (stdint.h HAVE_STDINT_H)
Check_Include_File (stdlib.h HAVE_STDLIB_H)
Check_Include_File (strings.h HAVE_STRINGS_H)
Check_Include_File (string.h HAVE_STRING_H)
Check_Include_File (sys/param.h HAVE_SYS_PARAM_H)
Check_Include_File (sys/types.h HAVE_SYS_types_H)
Check_Include_File (unistd.h HAVE_UNISTD_H)
Check_Include_File (sys/stat.h HAVE_SYS_STAT_H)
Check_Include_File (wchar.h HAVE_WCHAR_H)

Find_Path (LIBCWD_INCLUDE_DIR libcwd/sys.h DOC "Path to libcwd library") 
If (LIBCWD_INCLUDE_DIR)
	Get_Filename_Component (LIBCWD_PATH ${LIBCWD_INCLUDE_DIR} PATH CACHE)
	Find_Library (LIBCWD_LIBRARY NAMES cwd PATHS ${LIBCWD_PATH}/lib  NO_DEFAULT_PATH)
	Set (HAVE_LIBCWD_SYS_H 1 CACHE INTERNAL "Libcwd sys.h file presence")
Else (LIBCWD_INCLUDE_DIR)
	Set (HAVE_LIBCWD_SYS_H 0 CACHE INTERNAL "Libcwd sys.h file presence")
EndIf (LIBCWD_INCLUDE_DIR)

Mark_As_Advanced (LIBCWD_INCLUDE_DIR LIBCWD_LIBRARY LIBCWD_PATH)

Include (${CMAKE_MODULE_PATH}/Zlib.cmake)

Check_Symbol_Exists (truncate unistd.h HAVE_TRUNCATE)

Find_Bool (HAS_BOOL)

If (HAS_BOOL)
	Set (CXX_HAS_NO_BOOL 0 CACHE INTERNAL "Cxx Boolean Type existance")
Else (HAS_BOOL)
	Set (CXX_HAS_NO_BOOL 1 CACHE INTERNAL "Cxx Boolean Type existance")
EndIf (HAS_BOOL)

Set (config_h_file ${CMAKE_CURRENT_BINARY_DIR}/config.h)

Configure_File (
	${CMAKE_CURRENT_SOURCE_DIR}/config.h.cmake
	${config_h_file}
	)

Include_Directories (
	include
	include/id3
	${CMAKE_CURRENT_BINARY_DIR}
	${ZLIB_INCLUDE_DIR}
	)

Set (CMAKE_CXX_FLAGS_DEBUG "${CMAKE_CXX_FLAGS_DEBUG} -DID3_ENABLE_DEBUG")

If (HAVE_LIBCWD_SYS_H)
	Set (CMAKE_CXX_FLAGS_DEBUG "${CMAKE_CXX_FLAGS_DEBUG} -I${LIBCWD_INCLUDE_DIR}")
EndIf (HAVE_LIBCWD_SYS_H)

File (GLOB id3_h_files include/id3/*.h)

Add_Definitions ("-DHAVE_CONFIG_H")
Add_Subdirectory (src)
Add_Subdirectory (doc)
