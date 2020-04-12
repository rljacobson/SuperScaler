project( SuperScalar )
cmake_minimum_required( VERSION 3.1 )

set( CMAKE_CXX_STANDARD 14 )
add_definitions ( -Wall )
set( CMAKE_BUILD_TYPE Release )


## Qt Dependency
set( CMAKE_INCLUDE_CURRENT_DIR ON )
set( CMAKE_AUTOMOC ON )
set( CMAKE_PREFIX_PATH /usr/local/opt/qt/lib/cmake ${CMAKE_PREFIX_PATH})

find_package( Qt5Widgets REQUIRED )
find_package( Qt5Core REQUIRED )
find_package( Qt5Gui REQUIRED )
find_package( Qt5LinguistTools REQUIRED)
find_package(Qt5Multimedia REQUIRED)


## LibTrashcan Dependency
set(TRASHCAN_DIR ${PROJECT_SOURCE_DIR}/libtrashcan/src)
#add_library(trashcan ${TRASHCAN_DIR}/trashcan.c)


set ( SuperScalar_HDRS src/mainwindow.h ${TRASHCAN_DIR}/trashcan.h)
set ( SuperScalar_SRCS
		${TRASHCAN_DIR}/trashcan.c
		src/Anime4K.cpp
		src/CustomResolution.cpp
		src/checkupdate.cpp
		src/files.cpp
		src/gif.cpp
		src/main.cpp
		src/mainwindow.cpp
		src/progressBar.cpp
		src/settings.cpp
		src/srmd_ncnn_vulkan.cpp
		src/table.cpp
		src/textBrowser.cpp
		src/video.cpp
		src/waifu2x.cpp
		src/waifu2x_converter.cpp
		src/waifu2x_ncnn_vulkan.cpp
		${SuperScalar_HDRS}
	)
set (SuperScalar_UIS src/mainwindow.ui)
qt5_wrap_ui(UIS ${SuperScalar_UIS})
set (SuperScalar_RSCS
		src/icon.qrc
	)
qt5_add_resources(RSCS ${SuperScalar_RSCS})
set ( SuperScalar_TRS
		src/language_Chinese.ts
		src/language_English.ts
		src/language_Japanese.ts
	)
qt5_add_translation(TRS ${SuperScalar_TRS})

add_executable( SuperScalar ${SuperScalar_SRCS} ${UIS} ${RSCS} ${TRS} ${MOCS})

if(APPLE)
	find_library(COCOA COCOA)
	find_library(COREFOUNDATION CoreFoundation)
	target_include_directories(SuperScalar PUBLIC ${COCOA} ${COREFOUNDATION})
	target_link_libraries(SuperScalar ${COCOA} ${COREFOUNDATION})
endif()
target_include_directories(SuperScalar PUBLIC ${TRASHCAN_DIR})



target_link_libraries( SuperScalar
		Qt5::Core
		Qt5::Gui
		Qt5::Widgets
		Qt5::Multimedia
	)
