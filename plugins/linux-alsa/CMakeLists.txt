project(linux-alsa)

if(DISABLE_ALSA)
	message(STATUS "ALSA support disabled")
	return()
endif()

find_package(ALSA)

if(NOT ALSA_FOUND AND ALSA_PULSEAUDIO)
	message(FATAL_ERROR "ALSA not found but set as enabled")
elseif(NOT ALSA_FOUND)
	message(STATUS "ALSA not found, disabling ALSA plugin")
	return()
endif()

include_directories(
	SYSTEM "${CMAKE_SOURCE_DIR}/libobs"
	${ALSA_INCLUDE_DIR}
)

set(linux-alsa_SOURCES
	linux-alsa.c
	linux-alsa-input.c
	linux-alsa-output.c
)

add_library(linux-alsa MODULE
	${linux-alsa_SOURCES}
)
target_link_libraries(linux-alsa
	libobs
	${ALSA_LIBRARY}
)

install_obs_plugin_with_data(linux-alsa data)
