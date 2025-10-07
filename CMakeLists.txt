cmake_minimum_required(VERSION 3.22)
cmake_policy(SET CMP0048 NEW)
PROJECT(lmdb VERSION 0.9.70 LANGUAGES C)

cmake_minimum_required(VERSION 3.10)

INCLUDE(CheckSymbolExists)

#threads package
set(THREADS_PREFER_PTHREAD_FLAG ON)
find_package(Threads)

INCLUDE_DIRECTORIES(.)

#source files
FILE(GLOB LMDB_PUBLIC_HEADERS lmdb.h)
FILE(GLOB LMDB_HEADERS midl.h)
FILE(GLOB LMDB_SOURCES mdb.c midl.c)

ADD_LIBRARY(lmdb STATIC ${LMDB_SOURCES})
TARGET_COMPILE_OPTIONS(
    lmdb PRIVATE -fPIC -O2
    -Wno-unused-parameter -Wbad-function-cast -Wuninitialized)

ADD_LIBRARY(lmdb-${CMAKE_PROJECT_VERSION} SHARED ${LMDB_SOURCES})
TARGET_COMPILE_OPTIONS(
    lmdb-${CMAKE_PROJECT_VERSION} PRIVATE -fPIC -O2 -Wno-unused-parameter
    -Wbad-function-cast -Wuninitialized)

FILE(GLOB MDB_STAT_SOURCES mdb_stat.c)
ADD_EXECUTABLE(
    mdb_stat ${LMDB_SOURCES} ${MDB_STAT_SOURCES})
TARGET_COMPILE_OPTIONS(
    mdb_stat PRIVATE -O2 -Wno-unused-parameter -Wbad-function-cast
    -Wuninitialized)
TARGET_LINK_LIBRARIES(
    mdb_stat Threads::Threads)

FILE(GLOB MDB_COPY_SOURCES mdb_copy.c)
ADD_EXECUTABLE(
    mdb_copy ${LMDB_SOURCES} ${MDB_COPY_SOURCES})
TARGET_COMPILE_OPTIONS(
    mdb_copy PRIVATE -O2 -Wno-unused-parameter -Wbad-function-cast
    -Wuninitialized)
TARGET_LINK_LIBRARIES(
    mdb_copy Threads::Threads)

FILE(GLOB MDB_DUMP_SOURCES mdb_dump.c)
ADD_EXECUTABLE(
    mdb_dump ${LMDB_SOURCES} ${MDB_DUMP_SOURCES})
TARGET_COMPILE_OPTIONS(
    mdb_dump PRIVATE -O2 -Wno-unused-parameter -Wbad-function-cast
    -Wuninitialized)
TARGET_LINK_LIBRARIES(
    mdb_dump Threads::Threads)

FILE(GLOB MDB_LOAD_SOURCES mdb_load.c)
ADD_EXECUTABLE(
    mdb_load ${LMDB_SOURCES} ${MDB_LOAD_SOURCES})
TARGET_COMPILE_OPTIONS(
    mdb_load PRIVATE -O2 -Wno-unused-parameter -Wbad-function-cast
    -Wuninitialized)
TARGET_LINK_LIBRARIES(
    mdb_load Threads::Threads)

FILE(GLOB MDB_DROP_SOURCES mdb_drop.c)
ADD_EXECUTABLE(
    mdb_drop ${LMDB_SOURCES} ${MDB_DROP_SOURCES})
TARGET_COMPILE_OPTIONS(
    mdb_drop PRIVATE -O2 -Wno-unused-parameter -Wbad-function-cast
    -Wuninitialized)
TARGET_LINK_LIBRARIES(
    mdb_drop Threads::Threads)

FILE(GLOB MTEST_SOURCES mtest.c)
ADD_EXECUTABLE(
    mtest ${LMDB_SOURCES} ${MTEST_SOURCES})
TARGET_COMPILE_OPTIONS(
    mtest PRIVATE -O0 -g --coverage -Wno-unused-parameter -Wbad-function-cast
    -Wuninitialized)
TARGET_LINK_OPTIONS(
    mtest PRIVATE --coverage)
TARGET_LINK_LIBRARIES(
    mtest Threads::Threads)

FILE(GLOB MTEST2_SOURCES mtest2.c)
ADD_EXECUTABLE(
    mtest2 ${LMDB_SOURCES} ${MTEST2_SOURCES})
TARGET_COMPILE_OPTIONS(
    mtest2 PRIVATE -O0 -g --coverage -Wno-unused-parameter -Wbad-function-cast
    -Wuninitialized)
TARGET_LINK_OPTIONS(
    mtest2 PRIVATE --coverage)
TARGET_LINK_LIBRARIES(
    mtest2 Threads::Threads)

FILE(GLOB MTEST3_SOURCES mtest3.c)
ADD_EXECUTABLE(
    mtest3 ${LMDB_SOURCES} ${MTEST3_SOURCES})
TARGET_COMPILE_OPTIONS(
    mtest3 PRIVATE -O0 -g --coverage -Wno-unused-parameter -Wbad-function-cast
    -Wuninitialized)
TARGET_LINK_OPTIONS(
    mtest3 PRIVATE --coverage)
TARGET_LINK_LIBRARIES(
    mtest3 Threads::Threads)

FILE(GLOB MTEST4_SOURCES mtest4.c)
ADD_EXECUTABLE(
    mtest4 ${LMDB_SOURCES} ${MTEST4_SOURCES})
TARGET_COMPILE_OPTIONS(
    mtest4 PRIVATE -O0 -g --coverage -Wno-unused-parameter -Wbad-function-cast
    -Wuninitialized)
TARGET_LINK_OPTIONS(
    mtest4 PRIVATE --coverage)
TARGET_LINK_LIBRARIES(
    mtest4 Threads::Threads)

FILE(GLOB MTEST5_SOURCES mtest5.c)
ADD_EXECUTABLE(
    mtest5 ${LMDB_SOURCES} ${MTEST5_SOURCES})
TARGET_COMPILE_OPTIONS(
    mtest5 PRIVATE -O0 -g --coverage -Wno-unused-parameter -Wbad-function-cast
    -Wuninitialized)
TARGET_LINK_OPTIONS(
    mtest5 PRIVATE --coverage)
TARGET_LINK_LIBRARIES(
    mtest5 Threads::Threads)

ADD_CUSTOM_TARGET(
    test1 COMMAND rm -rf testdb && mkdir testdb && ./mtest && ./mdb_stat testdb
    DEPENDS mtest mdb_stat USES_TERMINAL)
ADD_CUSTOM_TARGET(
    test2 COMMAND rm -rf testdb && mkdir testdb && ./mtest2 && ./mdb_stat testdb
    DEPENDS mtest2 mdb_stat USES_TERMINAL)
ADD_CUSTOM_TARGET(
    test3 COMMAND rm -rf testdb && mkdir testdb && ./mtest3 && ./mdb_stat testdb
    DEPENDS mtest3 mdb_stat USES_TERMINAL)
ADD_CUSTOM_TARGET(
    test4 COMMAND rm -rf testdb && mkdir testdb && ./mtest4 && ./mdb_stat testdb
    DEPENDS mtest4 mdb_stat USES_TERMINAL)
ADD_CUSTOM_TARGET(
    test5 COMMAND rm -rf testdb && mkdir testdb && ./mtest5 && ./mdb_stat testdb
    DEPENDS mtest5 mdb_stat USES_TERMINAL)

ADD_CUSTOM_TARGET(
    test
    COMMAND echo "Tests complete."
    DEPENDS test1 test2 test3 test4 test4
    USES_TERMINAL)

#Build a pkg-config file
SET(LMDB_PC "${CMAKE_BINARY_DIR}/lmdb.pc")
FILE(WRITE  ${LMDB_PC} "Name: lmdb")
FILE(APPEND ${LMDB_PC} "\nDescription: LMDB database library")
FILE(APPEND ${LMDB_PC} "\nVersion: ${CMAKE_PROJECT_VERSION}")
FILE(APPEND ${LMDB_PC} "\nURL: https://github.com/incept5/lmdb")
FILE(APPEND ${LMDB_PC} "\nprefix=\${pcfiledir}/../..")
FILE(APPEND ${LMDB_PC} "\nlibdir=\${prefix}/lib")
FILE(APPEND ${LMDB_PC} "\nincludedir=\${prefix}/include")
FILE(APPEND ${LMDB_PC} "\nLibs: -L\${libdir} -llmdb")
FILE(APPEND ${LMDB_PC} "\nCflags: -I\${includedir}")
INSTALL(FILES ${LMDB_PC} DESTINATION lib/pkgconfig)

#install headers
INSTALL(FILES ${LMDB_PUBLIC_HEADERS} DESTINATION include)

#Install libraries
INSTALL(TARGETS lmdb lmdb-${CMAKE_PROJECT_VERSION}
        LIBRARY DESTINATION lib
        ARCHIVE DESTINATION lib)

#Install binaries
INSTALL(TARGETS mdb_stat mdb_copy mdb_dump mdb_load mdb_drop
        RUNTIME DESTINATION bin)

#Install man pages
INSTALL(FILES mdb_stat.1 mdb_copy.1 mdb_dump.1 mdb_load.1 mdb_drop.1
        DESTINATION man/man1)
