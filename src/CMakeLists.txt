project(test)
cmake_minimum_required(VERSION 2.8)

add_definitions( -DBUILD_AUDIO )

add_library(LIBUSB SHARED IMPORTED)
set_target_properties(LIBUSB PROPERTIES
  IMPORTED_LOCATION ${LIBUSB_1_LIBRARY}
  LINKER_LANGUAGE C)
#  IMPORTED_LOCATION /home/semi/workspace/proj1/libs/armeabi/libusb.so

# add_library(LIBM SHARED IMPORTED)
# set_target_properties(LIBM PROPERTIES
#   IMPORTED_LOCATION /home/semi/workspace/c/tegra-ftdi/tegra-libs/libm.so
#   LINKER_LANGUAGE C)

# include_directories(../include /home/semi/workspace/c/tegra-ftdi/libusb-1.0/ .)
include_directories(../include ../wrappers/c_sync/ ${LIBUSB_1_INCLUDE_DIR} ./)
# add_library(freenect SHARED ./freenect_internal.h ./audio.c ./cameras.h ./core.c ./loader.c ./registration.c ./tilt.c ./cameras.c ./usb_libusb10.c)
add_library(freenect SHARED ./audio.c ./cameras.h ./core.c ./loader.c ./registration.c ./tilt.c ./cameras.c ./usb_libusb10.c ../wrappers/c_sync/libfreenect_sync.c)
# target_link_libraries(freenect LIBM LIBUSB)
target_link_libraries(freenect LIBUSB)

add_executable(micview ../examples/micview.c)
target_link_libraries(micview freenect LIBUSB)

add_executable(regtest ../examples/regtest.c)
target_link_libraries(regtest freenect LIBUSB)

add_executable(tiltdemo ../examples/tiltdemo.c)
target_link_libraries(tiltdemo freenect LIBUSB)
