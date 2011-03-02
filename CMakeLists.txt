Project (ID3Lib)
Cmake_Minimum_Required (VERSION 2.8.4)

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

Set (CMAKE_MODULES_DIR ${CMAKE_CURRENT_SOURCE_DIR}/CMake)

Include (FindZLIB)
Include (CheckIncludeFile)
Include (CheckIncludeFiles)
Include (CheckIncludeFileCXX)
Include (CheckSymbolExists)
Include (${CMAKE_MODULES_DIR}/Find.cmake)

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

Set (CMAKE_REQUIRED_INCLUDES ${ZLIB_INCLUDE_DIR})
Check_Include_File (zlib.h HAVE_ZLIB_H)

Check_Symbol_Exists (truncate unistd.h HAVE_TRUNCATE)

Find_Bool (HAS_BOOL)

If (HAS_BOOL)
	Set (CXX_HAS_NO_BOOL 0 CACHE INTERNAL "Cxx Boolean Type existance")
Else (HAS_BOOL)
	Set (CXX_HAS_NO_BOOL 1 CACHE INTERNAL "Cxx Boolean Type existance")
EndIf (HAS_BOOL)

Configure_File (
	${CMAKE_CURRENT_SOURCE_DIR}/config.h.cmake
	${CMAKE_CURRENT_BINARY_DIR}/config.h
	)

Include_Directories (
	include
	include/id3
	${CMAKE_CURRENT_BINARY_DIR}
	${ZLIB_INCLUDE_DIR}
	)

Add_Definitions ("-DHAVE_CONFIG_H")
Add_Subdirectory (src)
