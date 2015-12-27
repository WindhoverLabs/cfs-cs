include(CTest)
find_program( MEMORYCHECK_COMMAND valgrind )
find_package(Threads)
set( MEMORYCHECK_COMMAND_OPTIONS "--trace-children=yes --leak-check=full" )

# Set include directories
include_directories(
	../mission_inc
	../platform_inc
	$ENV{CFE_DIR}/fsw/mission_inc
	$ENV{CFE_DIR}/fsw/platform_inc/cpu1
	${CMAKE_CURRENT_SOURCE_DIR}
	${CFE_INC_DIRS}
	${OSAL_INC_DIRS}
	${UTF_INCS}
	${APP_INC_DIRS})

add_definitions(-DCFE_LINUX)

set(CMAKE_C_FLAGS "-g -O0 -Wformat=0 -Wno-pointer-to-int-cast -Wno-int-to-pointer-cast -fprofile-arcs -ftest-coverage")

# Add the executables
add_executable(CS-UT 
		${CMAKE_CURRENT_SOURCE_DIR}/utf_test_cs.c
		${APP_SRC}
		${UTF_SRC})
target_link_libraries(CS-UT -fprofile-arcs gcov ${CMAKE_THREAD_LIBS_INIT})

# We need to both run the test and compare the as-run with the expected output
# with a diff command. 
add_test(NAME cs-ut COMMAND $<TARGET_FILE:CS-UT>)

# Copy input files to build directory
configure_file(${CMAKE_CURRENT_SOURCE_DIR}/cs_utf_cmds.in ${CMAKE_CURRENT_BINARY_DIR} COPYONLY)
    
# Build tables for the tests
add_subdirectory(../tables cf/apps/DefaultTable)
add_subdirectory(cf/apps/CorrFullTable cf/apps/CorrFullTable)
add_subdirectory(cf/apps/CorrNonFullTable cf/apps/CorrNonFullTable)
add_subdirectory(cf/apps/CorrQQTable cf/apps/CorrQQTable)
add_subdirectory(cf/apps/DupEntryNonFullTable cf/apps/DupEntryNonFullTable)
add_subdirectory(cf/apps/InvldEmptyNonFullTable cf/apps/InvldEmptyNonFullTable)
add_subdirectory(cf/apps/InvldNameNonFullTable cf/apps/InvldNameNonFullTable)
add_subdirectory(cf/apps/InvldStNonFullTable cf/apps/InvldStNonFullTable)



