# QtWebApp Template Engine Module

set(TEMPLATEENGINE_HEADERS
    templateglobal.h
    template.h
    templateloader.h
    templatecache.h
)

set(TEMPLATEENGINE_SOURCES
    template.cpp
    templateloader.cpp
    templatecache.cpp
)

target_sources(QtWebApp PRIVATE
    ${TEMPLATEENGINE_SOURCES}
    ${TEMPLATEENGINE_HEADERS}
)

# Add Core5Compat if available (optional dependency)
if(TARGET Qt6::Core5Compat)
    target_link_libraries(QtWebApp PUBLIC Qt6::Core5Compat)
endif()
