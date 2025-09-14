# QtWebApp HTTP Server Module

set(HTTPSERVER_HEADERS
    httpglobal.h
    httplistener.h
    httpconnectionhandler.h
    httpconnectionhandlerpool.h
    httprequest.h
    httpresponse.h
    httpcookie.h
    httprequesthandler.h
    httpsession.h
    httpsessionstore.h
    staticfilecontroller.h
)

set(HTTPSERVER_SOURCES
    httpglobal.cpp
    httplistener.cpp
    httpconnectionhandler.cpp
    httpconnectionhandlerpool.cpp
    httprequest.cpp
    httpresponse.cpp
    httpcookie.cpp
    httprequesthandler.cpp
    httpsession.cpp
    httpsessionstore.cpp
    staticfilecontroller.cpp
)

target_sources(QtWebApp PRIVATE
    ${HTTPSERVER_SOURCES}
    ${HTTPSERVER_HEADERS}
)

# Enable debug messages for debug builds
target_compile_definitions(QtWebApp PRIVATE
    $<$<CONFIG:Debug>:SUPERVERBOSE>
)
