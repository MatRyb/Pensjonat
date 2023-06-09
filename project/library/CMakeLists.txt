cmake_minimum_required(VERSION 3.4)
project(Library)

set(SOURCE_FILES

        #TU WSTAWIAMY KLASY
        src/model/Client.cpp include/model/Client.h
		src/model/Address.cpp include/model/Address.h
		src/model/Rent.cpp include/model/Rent.h src/model/Room.cpp include/model/Room.h src/model/ClientType.cpp include/model/ClientType.h src/model/Repository.cpp include/model/Repository.h include/exceptions/LogicException.h include/exceptions/ParameterException.h src/managers/ClientManager.cpp include/managers/ClientManager.h src/model/typedefs.cpp include/model/typedefs.h src/managers/RoomManager.cpp include/managers/RoomManager.h src/managers/RentManager.cpp include/managers/RentManager.h src/interface/InterfaceTerminal.cpp include/interface/InterfaceTerminal.h)
#Utwórz bibliotekę typu STATIC, SHARED albo MODULE ze wskazanych źródeł
add_library(Library ${SOURCE_FILES})

target_include_directories(Library PUBLIC include)
enable_testing()

find_package (Boost 1.60.0 COMPONENTS "unit_test_framework" REQUIRED)

include_directories(
        ${CMAKE_CURRENT_SOURCE_DIR}/include
        ${Boost_INCLUDE_DIRS}
)

set(SOURCE_TEST_FILES
		test/AddressTest.cpp test/master.cpp test/ClientManagerTest.cpp test/ClientTest.cpp test/ClientTypeTest.cpp test/RentManagerTest.cpp test/RentTest.cpp test/RepositoryTest.cpp test/RoomManagerTest.cpp test/RoomTest.cpp)

add_executable (LibraryTest ${SOURCE_TEST_FILES})

#dołącz bibliotekę z testowanymi klasami oraz bibliotekę boost.test do programu testów
target_link_libraries (LibraryTest
        Library
        ${Boost_UNIT_TEST_FRAMEWORK_LIBRARY}
        )

#dodaj test TestBiblioteki1. Instrukcji cmake add_test może być wiele.
#Dodatkowe parametry wpływają na szczegółowość generowanego raportu. Standardowo znajduje się on w Testing/Temporary.
add_test(LibraryTest1 LibraryTest
        --report_level=detailed
        --log_level=all
        --color_output=yes
        )

add_custom_target(check ${CMAKE_COMMAND} -E env CTEST_OUTPUT_ON_FAILURE=1 BOOST_TEST_LOG_LEVEL=all
        ${CMAKE_CTEST_COMMAND} -C $<CONFIGURATION> --verbose
        WORKING_DIRECTORY ${CMAKE_BINARY_DIR})
