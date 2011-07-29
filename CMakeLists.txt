# Copyright (C) 2007-2009 LuaDist.
# Created by Peter Kapec
# Redistribution and use of this file is allowed according to the terms of the MIT license.
# For details see the COPYRIGHT file distributed with LuaDist.
# Please note that the package source code is licensed under its own license.

# Project
	PROJECT(sqlite3 C)
	CMAKE_MINIMUM_REQUIRED(VERSION 2.6)
	INCLUDE(dist.cmake)

# SQLite configuration
	FIND_PACKAGE(Threads)
	IF(Threads_FOUND)
		IF(CMAKE_USE_PTHREADS_INIT)
			ADD_DEFINITIONS(-D_REENTRANT=1 -DTHREADSAFE=1)
			SET(EXTRA_LIBS ${CMAKE_THREAD_LIBS_INIT} dl)
		ENDIF()
	ENDIF()
	
	FIND_PACKAGE(Readline)
	IF(READLINE_FOUND)
		INCLUDE_DIRECTORIES(${READLINE_INCLUDE_DIRS})
	ENDIF()	

# SQLite Library
	ADD_LIBRARY(sqlite3 SHARED sqlite3.c )
	IF(CMAKE_USE_PTHREADS_INIT)	
		TARGET_LINK_LIBRARIES(sqlite3 ${EXTRA_LIBS})
	ENDIF()
	
	INSTALL(TARGETS sqlite3 RUNTIME DESTINATION ${INSTALL_BIN} LIBRARY DESTINATION ${INSTALL_LIB} ARCHIVE DESTINATION ${INSTALL_LIB})

# Install Headers
	INSTALL(FILES sqlite3.h sqlite3ext.h DESTINATION ${INSTALL_INC})
	
# SQLite client
	ADD_EXECUTABLE(sqlite-app shell.c)
	SET_TARGET_PROPERTIES(sqlite-app PROPERTIES OUTPUT_NAME sqlite CLEAN_DIRECT_OUTPUT 1 )	
	TARGET_LINK_LIBRARIES(sqlite-app sqlite3)	
	IF(READLINE_LIBRARIES)
		TARGET_LINK_LIBRARIES(sqlite-app ${READLINE_LIBRARIES})	
	ENDIF()
	INSTALL(TARGETS sqlite-app RUNTIME DESTINATION ${INSTALL_BIN} LIBRARY DESTINATION ${INSTALL_LIB} ARCHIVE DESTINATION ${INSTALL_LIB})