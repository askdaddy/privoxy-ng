cmake_minimum_required(VERSION 3.8)
project(privoxy)

set(CMAKE_CXX_STANDARD 11)

if(APPLE)
    set(CMAKE_SHARED_LIBRARY_CREATE_C_FLAGS "${CMAKE_SHARED_LIBRARY_CREATE_C_FLAGS} -undefined dynamic_lookup")
endif(APPLE)

configure_file(config.h.in config.h)

set(SOURCE_FILES
        pcre/chartables.c
        pcre/config.h
#        pcre/dftables.c
        pcre/get.c
        pcre/internal.h
        pcre/maketables.c
        pcre/pcre.c
        pcre/pcre.h
#        pcre/pcregrep.c
        pcre/pcreposix.c
        pcre/pcreposix.h
#        pcre/pcretest.c
        pcre/study.c
#        acconfig.h
        actionlist.h
        actions.c
        actions.h
        amiga.c
        amiga.h
        config.h
        cgi.c
        cgi.h
        cgiedit.c
        cgiedit.h
        cgisimple.c
        cgisimple.h
        client-tags.c
        client-tags.h
        cygwin.h
        deanimate.c
        deanimate.h
        encode.c
        encode.h
        errlog.c
        errlog.h
        filters.c
        filters.h
        gateway.c
        gateway.h
        jbsockets.c
        jbsockets.h
        jcc.c
        jcc.h
        list.c
        list.h
        loadcfg.c
        loadcfg.h
        loaders.c
        loaders.h
        miscutil.c
        miscutil.h
        parsers.c
        parsers.h
        pcrs.c
        pcrs.h
        project.h
        ssplit.c
        ssplit.h
        strptime.h
        urlmatch.c
        urlmatch.h
#        w32log.c
#        w32log.h
#        w32res.h
#        w32svrapi.c
#        w32svrapi.h
#        w32taskbar.c
#        w32taskbar.h
#        win32.c
#        win32.h
        )

add_executable(privoxy ${SOURCE_FILES})

find_package (Zlib)
message([STATUS] "find Zlib: ${ZLIB_FOUND}")
if (ZLIB_FOUND)
    include_directories(${ZLIB_INCLUDE_DIRS})
    message("Zlib include dirs: ${ZLIB_INCLUDE_DIRS}")
    target_link_libraries (privoxy ${ZLIB_LIBRARIES})
endif (ZLIB_FOUND)