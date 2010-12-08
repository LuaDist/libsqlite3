# Copyright (C) 2007-2009 LuaDist.
# Created by Peter Kapec
# Redistribution and use of this file is allowed according to the terms of the MIT license.
# For details see the COPYRIGHT file distributed with LuaDist.
# Please note that the package source code is licensed under its own license.

PROJECT(sqlite C)
CMAKE_MINIMUM_REQUIRED(VERSION 2.6)
INCLUDE(dist.cmake)

# SQLITE Library
SET(SQLITE_SRCS sqlite3.c libsqlite.def)

ADD_LIBRARY(sqlite SHARED ${SQLITE_SRCS} ${DEF_FILE})

INSTALL(TARGETS sqlite RUNTIME DESTINATION ${INSTALL_BIN} LIBRARY DESTINATION ${INSTALL_LIB} ARCHIVE DESTINATION ${INSTALL_LIB})

# Install Headers
INSTALL(FILES sqlite3.h sqlite3ext.h DESTINATION ${INSTALL_INC})
