INCLUDE_DIRECTORIES(
	${CMAKE_SOURCE_DIR}
	${CMAKE_SOURCE_DIR}/scribus
	${POPPLER_INCLUDE_DIR}
)


SET(IMPORTPDF_PLUGIN_UI_SRC
	pdfimportoptions.ui
)

SET(IMPORTPDF_PLUGIN_MOC_CLASSES
	importpdf.h
	importpdfplugin.h
	pdfimportoptions.h
)

SET(IMPORTPDF_PLUGIN_SOURCES
	importpdf.cpp
	importpdfplugin.cpp
	pdfimportoptions.cpp
	slaoutput.cpp
)

IF(HAVE_POPPLER)
	IF(HAVE_LCMS1)
		ADD_DEFINITIONS(-DUSE_CMS)
	ENDIF(HAVE_LCMS1)
	ADD_DEFINITIONS(-DHAVE_FREETYPE_H)
ENDIF(HAVE_POPPLER)

SET(SCRIBUS_IMPORTPDF_PLUGIN "importpdf")

QT5_WRAP_UI(IMPORTPDF_PLUGIN_UI_SOURCES ${IMPORTPDF_PLUGIN_UI_SRC} )
QT5_WRAP_CPP(IMPORTPDF_PLUGIN_MOC_SOURCES ${IMPORTPDF_PLUGIN_MOC_CLASSES})

ADD_LIBRARY(${SCRIBUS_IMPORTPDF_PLUGIN} MODULE ${IMPORTPDF_PLUGIN_SOURCES} ${IMPORTPDF_PLUGIN_MOC_SOURCES} ${IMPORTPDF_PLUGIN_UI_SOURCES})

LINK_DIRECTORIES( /usr/local/lib )
TARGET_LINK_LIBRARIES(${SCRIBUS_IMPORTPDF_PLUGIN} ${PLUGIN_LIBRARIES} ${POPPLER_LIBRARY})

INSTALL(TARGETS ${SCRIBUS_IMPORTPDF_PLUGIN}
	LIBRARY
	DESTINATION ${PLUGINDIR}
	PERMISSIONS ${PLUGIN_PERMISSIONS}
)

ADD_DEPENDENCIES(${SCRIBUS_IMPORTPDF_PLUGIN} ${EXE_NAME})

# SET_TARGET_PROPERTIES(${SCRIBUS_FONTPREVIEW_PLUGIN} PROPERTIES VERSION "0.0.0")
