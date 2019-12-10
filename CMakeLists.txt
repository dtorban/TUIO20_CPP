# This file is part of the MinVR cmake build system.  
# See the main MinVR/CMakeLists.txt file for authors, copyright, and license info.
#
# Code author(s):
# 		Dan Orban (dtorban)
#

cmake_minimum_required (VERSION 2.8)
project (TUIO2)

if(NOT CMAKE_DEBUG_POSTFIX)
  set(CMAKE_DEBUG_POSTFIX "d")
endif()
set(CMAKE_RELEASE_POSTFIX "")
set(CMAKE_RELWITHDEBINFO_POSTFIX "rd")
set(CMAKE_MINSIZEREL_POSTFIX "s")

if (CMAKE_BUILD_TYPE MATCHES "Release")
  set(CMAKE_BUILD_POSTFIX "${CMAKE_RELEASE_POSTFIX}")
elseif (CMAKE_BUILD_TYPE MATCHES "MinSizeRel")
  set(CMAKE_BUILD_POSTFIX "${CMAKE_MINSIZEREL_POSTFIX}")
elseif (CMAKE_BUILD_TYPE MATCHES "RelWithDebInfo")
  set(CMAKE_BUILD_POSTFIX "${CMAKE_RELWITHDEBINFO_POSTFIX}")
elseif (CMAKE_BUILD_TYPE MATCHES "Debug")
  set(CMAKE_BUILD_POSTFIX "${CMAKE_DEBUG_POSTFIX}")
else()
  set(CMAKE_BUILD_POSTFIX "")
endif()

set(SOURCEFILES 

  TUIO2/TuioBounds.cpp      
  TUIO2/TuioObject.cpp   
  TUIO2/TuioTime.cpp
  TUIO2/FlashSender.cpp  
  TUIO2/TuioBounds.h        
  TUIO2/TuioObject.h     
  TUIO2/TuioTime.h
  TUIO2/FlashSender.h    
  TUIO2/TuioClient.cpp      
  TUIO2/TuioPoint.cpp    
  TUIO2/TuioToken.cpp
  TUIO2/LibExport.h      
  TUIO2/TuioClient.h        
  TUIO2/TuioPointer.cpp  
  TUIO2/TuioToken.h
  TUIO2/OscReceiver.cpp  
  TUIO2/TuioComponent.cpp   
  TUIO2/TuioPointer.h    
  TUIO2/UdpReceiver.cpp
  TUIO2/OscReceiver.h    
  TUIO2/TuioComponent.h     
  TUIO2/TuioPoint.h      
  TUIO2/UdpReceiver.h
  TUIO2/OscSender.h      
  TUIO2/TuioDispatcher.cpp  
  TUIO2/TuioServer.cpp   
  TUIO2/UdpSender.cpp
  TUIO2/TcpReceiver.cpp  
  TUIO2/TuioDispatcher.h    
  TUIO2/TuioServer.h     
  TUIO2/UdpSender.h
  TUIO2/TcpReceiver.h    
  TUIO2/TuioListener.h      
  TUIO2/TuioSource.h     
  TUIO2/WebSockSender.cpp
  TUIO2/TcpSender.cpp    
  TUIO2/TuioManager.cpp     
  TUIO2/TuioSymbol.cpp   
  TUIO2/WebSockSender.h
  TUIO2/TcpSender.h      
  TUIO2/TuioManager.h       
  TUIO2/TuioSymbol.h


  oscpack/ip/IpEndpointName.cpp
  oscpack/ip/IpEndpointName.h
  oscpack/ip/NetworkingUtils.h
  oscpack/ip/PacketListener.h
  oscpack/ip/TimerListener.h
  oscpack/ip/UdpSocket.h

  oscpack/osc/MessageMappingOscPacketListener.h
  oscpack/osc/OscException.h
  oscpack/osc/OscHostEndianness.h
  oscpack/osc/OscOutboundPacketStream.cpp
  oscpack/osc/OscOutboundPacketStream.h
  oscpack/osc/OscPacketListener.h
  oscpack/osc/OscPrintReceivedElements.cpp
  oscpack/osc/OscPrintReceivedElements.h
  oscpack/osc/OscReceivedElements.cpp
  oscpack/osc/OscReceivedElements.h
  oscpack/osc/OscTypes.cpp
  oscpack/osc/OscTypes.h
)

if (${CMAKE_SYSTEM_NAME} MATCHES "Windows")
  set(SOURCEFILES ${SOURCEFILES}
    oscpack/ip/win32/NetworkingUtils.cpp
    oscpack/ip/win32/UdpSocket.cpp
  )
else()
  set(SOURCEFILES ${SOURCEFILES}
    oscpack/ip/posix/NetworkingUtils.cpp
    oscpack/ip/posix/UdpSocket.cpp
  )
endif()  

set (CMAKE_VERBOSE_MAKEFILE TRUE)

# Used for compiling in subdirectories of the project
include_directories (
  .
  oscpack
)


############################################################
#OS Specific flags should go here
############################################################

if (${CMAKE_SYSTEM_NAME} MATCHES "Darwin")
  set(CMAKE_XCODE_ATTRIBUTE_CLANG_CXX_LANGUAGE_STANDARD "c++11")
  set(CMAKE_XCODE_ATTRIBUTE_CLANG_CXX_LIBRARY "libc++")
  #set(CMAKE_CXX_FLAGS "${CMAKE_C_FLAGS} -std=c++11 -stdlib=libc++")
endif()

if (${CMAKE_SYSTEM_NAME} MATCHES "Linux")
  # Linux specific code
  set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -fPIC -std=c++11") 
  add_definitions("-DOSC_HOST_LITTLE_ENDIAN")
endif (${CMAKE_SYSTEM_NAME} MATCHES "Linux")

if (${CMAKE_SYSTEM_NAME} MATCHES "Windows")
  # Windows specific code
endif (${CMAKE_SYSTEM_NAME} MATCHES "Windows")

############################################################
#Finally add all the targets here
############################################################

add_library(TUIO2 ${SOURCEFILES})

#------------------------------------------
# Install Target
#------------------------------------------

install( TARGETS ${PROJECT_NAME}
         LIBRARY DESTINATION ${CMAKE_INSTALL_PREFIX}/lib
         ARCHIVE DESTINATION ${CMAKE_INSTALL_PREFIX}/lib
         RUNTIME DESTINATION ${CMAKE_INSTALL_PREFIX}/bin)

install(DIRECTORY ${PROJECT_SOURCE_DIR}/TUIO2/ DESTINATION "${CMAKE_INSTALL_PREFIX}/include/TUIO2")
install(DIRECTORY ${PROJECT_SOURCE_DIR}/oscpack/ip DESTINATION "${CMAKE_INSTALL_PREFIX}/include/TUIO2")
install(DIRECTORY ${PROJECT_SOURCE_DIR}/oscpack/osc DESTINATION "${CMAKE_INSTALL_PREFIX}/include/TUIO2")

