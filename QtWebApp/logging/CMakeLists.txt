# QtWebApp Logging Module

set(LOGGING_HEADERS
    logglobal.h
    logmessage.h
    logger.h
    filelogger.h
    dualfilelogger.h
)

set(LOGGING_SOURCES
    logmessage.cpp
    logger.cpp
    filelogger.cpp
    dualfilelogger.cpp
)

target_sources(QtWebApp PRIVATE
    ${LOGGING_SOURCES}
    ${LOGGING_HEADERS}
)
