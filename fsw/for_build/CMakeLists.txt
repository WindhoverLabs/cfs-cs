# Set local variables
set(APPTARGET cs)

# Set source files
set(APP_SRC 
	${CMAKE_CURRENT_SOURCE_DIR}/../src/cs_app_cmds.c
	${CMAKE_CURRENT_SOURCE_DIR}/../src/cs_app.c
	${CMAKE_CURRENT_SOURCE_DIR}/../src/cs_cmds.c
	${CMAKE_CURRENT_SOURCE_DIR}/../src/cs_compute.c
	${CMAKE_CURRENT_SOURCE_DIR}/../src/cs_eeprom_cmds.c
	${CMAKE_CURRENT_SOURCE_DIR}/../src/cs_memory_cmds.c
	${CMAKE_CURRENT_SOURCE_DIR}/../src/cs_table_cmds.c
	${CMAKE_CURRENT_SOURCE_DIR}/../src/cs_table_processing.c
	${CMAKE_CURRENT_SOURCE_DIR}/../src/cs_utils.c)
	
# Set include paths
set(APP_INC_DIRS 
	${CMAKE_CURRENT_SOURCE_DIR}/../src/)
include_directories(${APP_INC_DIRS})
include_directories(${CFE_INC_DIRS})
include_directories(${OSAL_INC_DIRS})
include_directories(${PSP_INC_DIRS})
	
# Build application
add_app(${APP_FILE} SHARED ${APP_SRC})

# Build tables
add_app(cs_apptbl STATIC ${TABLE_DIR}/cs_apptbl.c)
add_app(cs_eepromtbl STATIC ${TABLE_DIR}/cs_eepromtbl.c)
add_app(cs_memorytbl STATIC ${TABLE_DIR}/cs_memorytbl.c)
add_app(cs_tablestbl STATIC ${TABLE_DIR}/cs_tablestbl.c)
set_target_properties(cs_apptbl PROPERTIES PREFIX "" SUFFIX ".tbl")
set_target_properties(cs_eepromtbl PROPERTIES PREFIX "" SUFFIX ".tbl")
set_target_properties(cs_memorytbl PROPERTIES PREFIX "" SUFFIX ".tbl")
set_target_properties(cs_tablestbl PROPERTIES PREFIX "" SUFFIX ".tbl")

add_subdirectory(../unit_test unit_test)

# Generate documentation
find_package(Doxygen)
if(DOXYGEN_FOUND)
    configure_file(${CMAKE_CURRENT_SOURCE_DIR}/user_doxy.in ${CMAKE_CURRENT_BINARY_DIR}/user_doxy @ONLY)
    configure_file(${CMAKE_CURRENT_SOURCE_DIR}/detail_doxy.in ${CMAKE_CURRENT_BINARY_DIR}/detail_doxy @ONLY)
    configure_file(${CMAKE_CURRENT_SOURCE_DIR}/codewalk_doxy.in ${CMAKE_CURRENT_BINARY_DIR}/codewalk_doxy @ONLY)
    add_custom_target(cs-docs
        COMMENT "Generating API documentation with Doxygen" VERBATIM
    	COMMAND mkdir -p ${CMAKE_BINARY_DIR}/docs/users_guide/html/${APPTARGET}/
    	COMMAND mkdir -p ${CMAKE_BINARY_DIR}/docs/users_guide/latex/${APPTARGET}/
    	COMMAND mkdir -p ${CMAKE_BINARY_DIR}/docs/detailed_design/html/${APPTARGET}/
    	COMMAND mkdir -p ${CMAKE_BINARY_DIR}/docs/detailed_design/latex/${APPTARGET}/
    	COMMAND mkdir -p ${CMAKE_BINARY_DIR}/docs/cwt/html/${APPTARGET}/
    	COMMAND mkdir -p ${CMAKE_BINARY_DIR}/docs/cwt/latex/${APPTARGET}/
        COMMAND ${DOXYGEN_EXECUTABLE} ${CMAKE_CURRENT_BINARY_DIR}/user_doxy
        COMMAND ${DOXYGEN_EXECUTABLE} ${CMAKE_CURRENT_BINARY_DIR}/detail_doxy
        COMMAND ${DOXYGEN_EXECUTABLE} ${CMAKE_CURRENT_BINARY_DIR}/codewalk_doxy
        #COMMAND make -C ${CMAKE_BINARY_DIR}/docs/users_guide/latex/${APPTARGET}
        #COMMAND make -C ${CMAKE_BINARY_DIR}/docs/detailed_design/latex/${APPTARGET}
        #COMMAND make -C ${CMAKE_BINARY_DIR}/docs/cwt/latex/${APPTARGET}
        WORKING_DIRECTORY ${CMAKE_BINARY_DIR}/
    )
endif(DOXYGEN_FOUND)
