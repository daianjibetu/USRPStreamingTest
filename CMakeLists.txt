cmake_minimum_required(VERSION 3.13)
project(usrptest)

set (CMAKE_CXX_STANDARD 17)

set(CMAKE_MODULE_PATH "${CMAKE_SOURCE_DIR}/cmake/Modules" ${CMAKE_MODULE_PATH})

set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -fno-common -Wall -Wextra -Wno-unused-parameter -std=c99 -Werror=implicit-function-declaration")

set(CMAKE_C_FLAGS_DEBUG "-ggdb3 -g3 -O0 -fsanitize=address")
set(CMAKE_C_FLAGS_MINSIZEREL "-Os -fsanitize=address")
set(CMAKE_C_FLAGS_RELWITHDEBINFO "-O3 -ggdb3 -g3")
set(CMAKE_C_FLAGS_RELEASE "-O3")

#find_package(UHD REQUIRED)

set(THREADS_PREFER_PTHREAD_FLAG ON)
find_package(Threads REQUIRED)

find_package(PkgConfig)
pkg_check_modules(UHD REQUIRED uhd)
#find_package(Boost)
find_package(Boost COMPONENTS system REQUIRED)

#message("## Boost_INCLUDE_DIRS: ${Boost_INCLUDE_DIRS}")  # 確認
#message("## Boost_LIBRARY_DIRS: ${Boost_LIBRARY_DIRS}")  # 確認
#message("## Boost_LIBRARIES: ${Boost_LIBRARIES}")  # 確認

#message("## UHD_INCLUDE_DIRS: ${UHD_INCLUDE_DIRS}")  # 確認
#message("## UHD_LIBRARY_DIRS: ${UHD_LIBRARY_DIRS}")  # 確認
#message("## UHD_LIBRARIES: ${UHD_LIBRARIES}")  # 確認


include_directories(
    ${Boost_INCLUDE_DIRS}
    ${UHD_INCLUDE_DIRS}
    )

add_executable(${PROJECT_NAME} usrp.c main.c)
#add_executable(${PROJECT_NAME} rx_samples_c.c)

#link_directories(${Boost_LIBRARY_DIRS})

target_link_directories(${PROJECT_NAME} PRIVATE 
${UHD_LIBRARY_DIRS}
${Boost_LIBRARY_DIRS}
)

target_link_libraries(${PROJECT_NAME} PRIVATE 
Threads::Threads
${UHD_LIBRARIES} 
${Boost_LIBRARIES}
)

target_link_libraries(${PROJECT_NAME} PUBLIC "-pthread")