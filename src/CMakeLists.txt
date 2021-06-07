cmake_minimum_required(VERSION 3.12)

set(OBJS CharDistribution.cpp JpCntx.cpp tables/LangBulgarianModel.cpp tables/LangCyrillicModel.cpp tables/LangGreekModel.cpp tables/LangHebrewModel.cpp tables/LangHungarianModel.cpp tables/LangThaiModel.cpp tables/LangArabicModel.cpp tables/LangDanishModel.cpp tables/LangEsperantoModel.cpp tables/LangFrenchModel.cpp tables/LangGermanModel.cpp tables/LangSpanishModel.cpp tables/LangTurkishModel.cpp tables/LangVietnameseModel.cpp nsBig5Prober.cpp nsCharSetProber.cpp nsEUCJPProber.cpp nsEUCKRProber.cpp nsEUCTWProber.cpp nsEscCharsetProber.cpp nsEscSM.cpp nsGB2312Prober.cpp nsHebrewProber.cpp nsLatin1Prober.cpp nsMBCSGroupProber.cpp nsMBCSSM.cpp nsSBCSGroupProber.cpp nsSBCharSetProber.cpp nsSJISProber.cpp nsUTF8Prober.cpp nsUniversalDetector.cpp chardet.cpp)

set(HEADERS ${CMAKE_CURRENT_SOURCE_DIR}/nsUniversalDetector.h ${CMAKE_CURRENT_SOURCE_DIR}/chardet.h ${CMAKE_SOURCE_DIR}/include/nscore.h ${CMAKE_SOURCE_DIR}/include/version.h ${CMAKE_SOURCE_DIR}/chardet-config.h)

if(NOT BUILD_SHARED_LIBS AND NOT BUILD_STATIC_LIBS)
    message(FATAL_ERROR "At least one type of library.")
endif()

if(BUILD_SHARED_LIBS)
    if (MSVC)
        set(DLL_RC ../dll.rc)
    endif()
    add_library(libchardet SHARED ${OBJS} ${DLL_RC})
    set_target_properties(libchardet PROPERTIES PUBLIC_HEADER "${HEADERS}")
    set(INST_TARGET libchardet)
    set(LINK_TARGET libchardet PARENT_SCOPE)
    set(LINK_TARGET libchardet)
endif()
if(BUILD_STATIC_LIBS)
    set(STATLIB_NAME libchardet)
    if (MSVC AND BUILD_SHARED_LIBS)
        set(STATLIB_NAME libchardet-static)
    endif()
    add_library(libchardet-static STATIC ${OBJS})
    set_target_properties(libchardet-static PROPERTIES OUTPUT_NAME ${STATLIB_NAME})
    list(APPEND INST_TARGET libchardet-static)
    if (NOT DEFINED LINK_TARGET)
        set(LINK_TARGET libchardet-static PARENT_SCOPE)
    endif()
endif()

if (NOT CMAKE_INSTALL_INCLUDEDIR)
    set(CMAKE_INSTALL_INCLUDEDIR include/chardet)
endif()

install(TARGETS ${INST_TARGET}
PUBLIC_HEADER DESTINATION ${CMAKE_INSTALL_INCLUDEDIR})
