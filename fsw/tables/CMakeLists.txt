# Build tables for the tests
SET(CMAKE_CXX_ARCHIVE_CREATE "<CMAKE_AR> <OBJECTS> ${CMAKE_CURRENT_BINARY_DIR}")
SET(CMAKE_C_ARCHIVE_CREATE "<CMAKE_AR> <OBJECTS> ${CMAKE_CURRENT_BINARY_DIR}")
add_library(cs_apptabl STATIC cs_apptbl.c)
add_library(cs_eepromtabl STATIC cs_eepromtbl.c)
add_library(cs_memorytabl STATIC cs_memorytbl.c)
add_library(cs_tablestabl STATIC cs_tablestbl.c)

set_target_properties(cs_apptabl PROPERTIES PREFIX "" SUFFIX ".tbl")
set_target_properties(cs_eepromtabl PROPERTIES PREFIX "" SUFFIX ".tbl")
set_target_properties(cs_memorytabl PROPERTIES PREFIX "" SUFFIX ".tbl")
set_target_properties(cs_tablestabl PROPERTIES PREFIX "" SUFFIX ".tbl")
