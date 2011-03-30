# Copyright (C) 2007-2009 LuaDist.
# Created by Peter Drahoš
# Redistribution and use of this file is allowed according to the terms of the MIT license.
# For details see the COPYRIGHT file distributed with LuaDist.
# Please note that the package source code is licensed under its own license.

PROJECT(md5 C)
CMAKE_MINIMUM_REQUIRED(VERSION 2.6)
INCLUDE(dist.cmake)

ADD_LUA_MODULE(core src/md5.c src/md5lib.c src/md5.def)
ADD_LUA_MODULE(des56 src/des56.c src/ldes56.c src/des56.def)

IF(BUILD_TESTING)
	# Lua Test
	ADD_LUA_TEST(tests/test.lua)
	
	# Binary Tests
	INCLUDE_DIRECTORIES(src)
	ADD_EXECUTABLE(fdemo tests/DES56/fdemo.c src/des56.c)
	ADD_TEST(fdemo fdemo -demo -c 1000)
	FIND_LIBRARY(CRYPT_LIB crypt)
	IF (CRYPT_LIB)
		ADD_EXECUTABLE(ftest tests/DES56/ftest.c src/des56.c)
		TARGET_LINK_LIBRARIES(ftest ${CRYPT_LIB})
		ADD_TEST(ftest1 ftest 4869207468657265 a820105412de3991 77129bd8c052e7)
		ADD_TEST(ftest2 ftest 1.21280ac -c 5000000 -k 1000000 -C 100000 -K 100000)
	ENDIF (CRYPT_LIB)
ENDIF(BUILD_TESTING)

INSTALL(TARGETS core DESTINATION ${INSTALL_CMOD}/md5)
INSTALL(TARGETS des56 DESTINATION ${INSTALL_CMOD})
INSTALL(FILES src/md5.lua DESTINATION ${INSTALL_LMOD})
INSTALL(FILES README DESTINATION ${INSTALL_DATA})
INSTALL(DIRECTORY doc/ DESTINATION ${INSTALL_DOC})
INSTALL(DIRECTORY tests/ DESTINATION ${INSTALL_TEST})
