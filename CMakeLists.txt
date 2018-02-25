cmake_minimum_required(VERSION 2.8)

include(HLEBcoinWallet.cmake)

project(${CN_PROJECT_NAME})

execute_process(COMMAND git log -1 --pretty=format:%h
WORKING_DIRECTORY ${CMAKE_SOURCE_DIR}
OUTPUT_VARIABLE GIT_REVISION)

set(CRYPTONOTE_LIB HLEBcoinWallet)

include_directories(${CMAKE_BINARY_DIR}
    src
    HLEBcoin/external
    HLEBcoin/include
    HLEBcoin/src)

find_package(Qt5Gui REQUIRED)
find_package(Qt5Widgets REQUIRED)

set(Boost_USE_STATIC_LIBS ON)
if(WIN32)
  set(Boost_USE_STATIC_RUNTIME OFF)
else(WIN32)
  set(Boost_USE_STATIC_RUNTIME ON)
endif(WIN32)

find_package(Boost 1.66 REQUIRED COMPONENTS date_time filesystem program_options regex serialization system thread chrono)
if ((${Boost_MAJOR_VERSION} EQUAL 1) AND (${Boost_MINOR_VERSION} EQUAL 54))
	message(SEND_ERROR "Boost version 1.54 is unsupported, more details are available here http://goo.gl/RrCFmA")
endif ()

include_directories(${Boost_INCLUDE_DIRS})
link_directories(${Boost_LIBRARY_DIRS})

set(VERSION "")
configure_file("HLEBcoin/src/version.h.in" "version.h")
configure_file("src/CryptoNoteWalletConfig.h.in" "CryptoNoteWalletConfig.h")

add_definitions(-DGIT_REVISION=\"${GIT_REVISION}\")

set(CMAKE_AUTOMOC ON)

set(CRYPTONOTE_SOURCES
  HLEBcoin/external/miniupnpc/connecthostport.c
  HLEBcoin/external/miniupnpc/igd_desc_parse.c
  HLEBcoin/external/miniupnpc/minisoap.c
  HLEBcoin/external/miniupnpc/miniupnpc.c
  HLEBcoin/external/miniupnpc/miniwget.c
  HLEBcoin/external/miniupnpc/minixml.c
  HLEBcoin/external/miniupnpc/portlistingparse.c
  HLEBcoin/external/miniupnpc/receivedata.c
  HLEBcoin/external/miniupnpc/upnpcommands.c
  HLEBcoin/external/miniupnpc/upnpreplyparse.c
  HLEBcoin/src/Common/Base58.cpp
  HLEBcoin/src/Common/CommandLine.cpp
  HLEBcoin/src/Common/Util.cpp
  HLEBcoin/src/Common/StringTools.cpp
  HLEBcoin/src/Common/JsonValue.cpp
  HLEBcoin/src/Common/ConsoleTools.cpp
  HLEBcoin/src/Common/MemoryInputStream.cpp
  HLEBcoin/src/Common/PathTools.cpp
  HLEBcoin/src/Common/StdInputStream.cpp
  HLEBcoin/src/Common/StdOutputStream.cpp
  HLEBcoin/src/Common/StreamTools.cpp
  HLEBcoin/src/Common/StringOutputStream.cpp
  HLEBcoin/src/Common/StringView.cpp
  HLEBcoin/src/Common/VectorOutputStream.cpp
  HLEBcoin/src/crypto/blake256.c
  HLEBcoin/src/crypto/chacha8.c
  HLEBcoin/src/crypto/crypto-ops-data.c
  HLEBcoin/src/crypto/crypto-ops.c
  HLEBcoin/src/crypto/crypto.cpp
  HLEBcoin/src/crypto/groestl.c
  HLEBcoin/src/crypto/hash-extra-blake.c
  HLEBcoin/src/crypto/hash-extra-groestl.c
  HLEBcoin/src/crypto/hash-extra-jh.c
  HLEBcoin/src/crypto/hash-extra-skein.c
  HLEBcoin/src/crypto/hash.c
  HLEBcoin/src/crypto/jh.c
  HLEBcoin/src/crypto/keccak.c
  HLEBcoin/src/crypto/oaes_lib.c
  HLEBcoin/src/crypto/random.c
  HLEBcoin/src/crypto/skein.c
  HLEBcoin/src/crypto/slow-hash.c
  HLEBcoin/src/crypto/slow-hash.cpp
  HLEBcoin/src/crypto/tree-hash.c
  HLEBcoin/src/CryptoNoteCore/BlockchainIndices.cpp
  HLEBcoin/src/CryptoNoteCore/BlockchainMessages.cpp
  HLEBcoin/src/CryptoNoteCore/BlockIndex.cpp
  HLEBcoin/src/CryptoNoteCore/CoreConfig.cpp
  HLEBcoin/src/CryptoNoteCore/CryptoNoteBasic.cpp
  HLEBcoin/src/CryptoNoteCore/CryptoNoteTools.cpp
  HLEBcoin/src/CryptoNoteCore/Currency.cpp
  HLEBcoin/src/CryptoNoteCore/MinerConfig.cpp
  HLEBcoin/src/CryptoNoteCore/Transaction.cpp
  HLEBcoin/src/CryptoNoteCore/Account.cpp
  HLEBcoin/src/CryptoNoteCore/Blockchain.cpp
  HLEBcoin/src/CryptoNoteCore/Checkpoints.cpp
  HLEBcoin/src/CryptoNoteCore/CryptoNoteBasicImpl.cpp
  HLEBcoin/src/CryptoNoteCore/Core.cpp
  HLEBcoin/src/CryptoNoteCore/CryptoNoteFormatUtils.cpp
  HLEBcoin/src/CryptoNoteCore/CryptoNoteSerialization.cpp
  HLEBcoin/src/CryptoNoteCore/Difficulty.cpp
  HLEBcoin/src/CryptoNoteCore/IBlock.cpp
  HLEBcoin/src/CryptoNoteCore/Miner.cpp
  HLEBcoin/src/CryptoNoteCore/TransactionExtra.cpp
  HLEBcoin/src/CryptoNoteCore/TransactionPool.cpp
  HLEBcoin/src/CryptoNoteCore/TransactionPrefixImpl.cpp
  HLEBcoin/src/CryptoNoteCore/TransactionUtils.cpp
  HLEBcoin/src/InProcessNode/InProcessNode.cpp
  HLEBcoin/src/InProcessNode/InProcessNodeErrors.cpp
  HLEBcoin/src/NodeRpcProxy/NodeErrors.cpp
  HLEBcoin/src/NodeRpcProxy/NodeRpcProxy.cpp
  HLEBcoin/src/P2p/NetNodeConfig.cpp
  HLEBcoin/src/Serialization/BinaryInputStreamSerializer.cpp
  HLEBcoin/src/Serialization/BinaryOutputStreamSerializer.cpp
  HLEBcoin/src/Serialization/JsonInputValueSerializer.cpp
  HLEBcoin/src/Serialization/JsonOutputStreamSerializer.cpp
  HLEBcoin/src/Serialization/KVBinaryInputStreamSerializer.cpp
  HLEBcoin/src/Serialization/KVBinaryOutputStreamSerializer.cpp
  HLEBcoin/src/Serialization/SerializationOverloads.cpp
  HLEBcoin/src/Transfers/BlockchainSynchronizer.cpp
  HLEBcoin/src/Transfers/SynchronizationState.cpp
  HLEBcoin/src/Transfers/TransfersConsumer.cpp
  HLEBcoin/src/Transfers/TransfersContainer.cpp
  HLEBcoin/src/Transfers/TransfersSubscription.cpp
  HLEBcoin/src/Transfers/TransfersSynchronizer.cpp
  HLEBcoin/src/Wallet/LegacyKeysImporter.cpp
  HLEBcoin/src/Wallet/WalletAsyncContextCounter.cpp
  HLEBcoin/src/Wallet/WalletErrors.cpp
  HLEBcoin/src/WalletLegacy/KeysStorage.cpp
  HLEBcoin/src/WalletLegacy/WalletLegacy.cpp
  HLEBcoin/src/WalletLegacy/WalletHelper.cpp
  HLEBcoin/src/WalletLegacy/WalletLegacySerializer.cpp
  HLEBcoin/src/WalletLegacy/WalletLegacySerialization.cpp
  HLEBcoin/src/WalletLegacy/WalletTransactionSender.cpp
  HLEBcoin/src/WalletLegacy/WalletUnconfirmedTransactions.cpp
  HLEBcoin/src/WalletLegacy/WalletUserTransactionsCache.cpp
  HLEBcoin/src/System/ContextGroup.cpp
  HLEBcoin/src/System/Event.cpp
  HLEBcoin/src/System/EventLock.cpp
  HLEBcoin/src/System/InterruptedException.cpp
  HLEBcoin/src/System/Ipv4Address.cpp
  HLEBcoin/src/System/TcpStream.cpp
  HLEBcoin/src/HTTP/HttpRequest.cpp
  HLEBcoin/src/HTTP/HttpParser.cpp
  HLEBcoin/src/HTTP/HttpParserErrorCodes.cpp
  HLEBcoin/src/HTTP/HttpResponse.cpp
  HLEBcoin/src/Rpc/HttpClient.cpp
  HLEBcoin/src/Rpc/JsonRpc.cpp
  HLEBcoin/src/P2p/NetNode.cpp
  HLEBcoin/src/P2p/LevinProtocol.cpp
  HLEBcoin/src/P2p/NetNodeConfig.cpp
  HLEBcoin/src/P2p/PeerListManager.cpp
  HLEBcoin/src/CryptoNoteProtocol/CryptoNoteProtocolHandler.cpp
  HLEBcoin/src/Logging/ILogger.cpp
  HLEBcoin/src/Logging/LoggerGroup.cpp
  HLEBcoin/src/Logging/CommonLogger.cpp
  HLEBcoin/src/Logging/LoggerManager.cpp
  HLEBcoin/src/Logging/FileLogger.cpp
  HLEBcoin/src/Logging/StreamLogger.cpp
  HLEBcoin/src/Logging/ConsoleLogger.cpp
  HLEBcoin/src/Logging/LoggerMessage.cpp
  HLEBcoin/src/Logging/LoggerRef.cpp
  HLEBcoin/src/BlockchainExplorer/BlockchainExplorer.cpp
  HLEBcoin/src/BlockchainExplorer/BlockchainExplorerDataBuilder.cpp
  HLEBcoin/src/BlockchainExplorer/BlockchainExplorerErrors.cpp
)

file(GLOB_RECURSE SOURCES src/*.cpp)
file(GLOB_RECURSE HEADERS src/*.h)
file(GLOB_RECURSE FORMS src/gui/ui/*.ui)

set(QRC src/resources.qrc)

qt5_wrap_ui(UIS ${FORMS})
qt5_add_resources(RCC ${QRC})


if (WIN32)
  if (NOT MSVC)
	message(FATAL_ERROR "Only MSVC is supported on this platform")
  endif ()
  add_definitions(/D_CRT_SECURE_NO_WARNINGS /D_WIN32_WINNT=0x0600 /DSTATICLIB)
  include_directories(HLEBcoin/src/platform/msc)

  set(PLATFORM_DIR Windows)
  set(BUILD_PLATFORM WIN32)
  set(BUILD_RESOURCES src/cryptonotewallet.rc)
  set(QTMAIN Qt5::WinMain)

elseif (UNIX)
  set(CRYPTONOTE_SOURCES ${CRYPTONOTE_SOURCES} HLEBcoin/external/miniupnpc/minissdpc.c)
  if (APPLE)
    enable_language(ASM)
    file(GLOB_RECURSE OBJC_SOURCES src/*.mm)
    set(SOURCES ${SOURCES} ${OBJC_SOURCES})
    set(PLATFORM_DIR OSX)
    set(MACOSX_BUNDLE_INFO_STRING "HLEBcoin GUI wallet")
    set(MACOSX_BUNDLE_LONG_VERSION_STRING "${VERSION_VERSION}.${VERSION_MINOR}.${VERSION_PATCH}")
    set(MACOSX_BUNDLE_BUNDLE_NAME HLEBcoinWallet)
    set(MACOSX_BUNDLE_SHORT_VERSION_STRING "${VERSION_VERSION}.${VERSION_MINOR}.${VERSION_PATCH}")
    set(MACOSX_BUNDLE_BUNDLE_VERSION "$${VERSION_VERSION}.${VERSION_MINOR}.${VERSION_PATCH}")

    find_package(Qt5PrintSupport REQUIRED)

    include_directories(/usr/include/malloc)

    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -maes -std=c++11 -stdlib=libc++")
    set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -std=c11 -maes -D_DARWIN_C_SOURCE")
    set(CMAKE_EXE_LINKER_FLAGS "${CMAKE_EXE_LINKER_FLAGS} -framework Cocoa -framework OpenGL -framework CoreFoundation -framework Carbon -framework IOKit -L/usr/lib")

    set(MACOSX_BUNDLE_ICON_FILE cryptonote.icns)
    set(APPLICATION_ICON src/images/cryptonote.icns)
    set_source_files_properties(${APPLICATION_ICON} PROPERTIES MACOSX_PACKAGE_LOCATION "Resources")

    set(BUILD_PLATFORM MACOSX_BUNDLE)
    set(BUILD_RESOURCES ${APPLICATION_ICON})
	
    GET_TARGET_PROPERTY(QT_LIB_DIR "${Qt5Widgets_LIBRARIES}" LOCATION)
    GET_FILENAME_COMPONENT(QT_LIB_DIR "${QT_LIB_DIR}" PATH)
  else(APPLE)
    set(PLATFORM_DIR Linux)
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -maes -std=c++11")
    set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -maes -std=c11")
  endif (APPLE)
endif ()

include_directories(HLEBcoin/src/Platform/${PLATFORM_DIR})
file(GLOB PLATFORM_SOURCES HLEBcoin/src/Platform/${PLATFORM_DIR}/System/*)
set(CRYPTONOTE_SOURCES ${CRYPTONOTE_SOURCES} ${PLATFORM_SOURCES})

add_library(${CRYPTONOTE_LIB} STATIC ${CRYPTONOTE_SOURCES})
set_target_properties(${CRYPTONOTE_LIB} PROPERTIES COMPILE_DEFINITIONS _GNU_SOURCE)
target_link_libraries(${CRYPTONOTE_LIB} ${Boost_LIBRARIES})

add_executable(${PROJECT_NAME} ${BUILD_PLATFORM} ${BUILD_RESOURCES} ${SOURCES} ${HEADERS} ${UIS} ${RCC})
set_target_properties(${PROJECT_NAME} PROPERTIES COMPILE_DEFINITIONS _GNU_SOURCE)
target_link_libraries(${PROJECT_NAME} ${Boost_LIBRARIES} ${QTMAIN} ${CRYPTONOTE_LIB})

if (APPLE)
  qt5_use_modules(${PROJECT_NAME} PrintSupport)
elseif (UNIX)
  target_link_libraries(${PROJECT_NAME} -lpthread)
elseif (WIN32)
  target_link_libraries(${PROJECT_NAME} Imm32 Iphlpapi Winmm)
endif (APPLE)

qt5_use_modules(${PROJECT_NAME} Widgets Gui)
