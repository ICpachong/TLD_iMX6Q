# 版本限定 
CMAKE_MINIMUM_REQUIRED(VERSION 2.8)     
# 项目名称 
PROJECT(IMX6Q_VPU_Capture_Module.out)
# 设置编译器选项：O3优化，显示所有警告
SET(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++0x -O2 -Wall -g")
SET(CMAKE_C_FLAGS "$ENV{CFLAGS} -O2 -Wall -g")

SET(CMAKE_SYSTEM_NAME Linux) 
SET(TOOLCHAIN_DIR "/home/xiangang/xiangang/arm-linux-gcc/gcc-4.6.2-glibc-2.13-linaro-multilib-2011.12/fsl-linaro-toolchain")
set(CMAKE_CXX_COMPILER ${TOOLCHAIN_DIR}/bin/arm-none-linux-gnueabi-g++) 
set(CMAKE_C_COMPILER   ${TOOLCHAIN_DIR}/bin/arm-none-linux-gnueabi-gcc)


# 定义头文件的路径变量
SET(INCLUDE_PATH ${PROJECT_SOURCE_DIR}/include) 

#头文件
include_directories(/home/xiangang/xiangang/ffmpeg/ffmpeglib/include)

# 定义库文件路径
SET(LIB_PATH ${PROJECT_SOURCE_DIR}/lib)
MESSAGE(STATUS "Libs Path, ${LIB_PATH}")

# 定义源文件路径变量
SET(SOURCE_PATH ${PROJECT_SOURCE_DIR}/src) 
MESSAGE(STATUS "Source Path , ${SOURCE_PATH}") 

# 包含头文件搜索路径
#INCLUDE_DIRECTORIES(${INCLUDE_PATH}) 

# 包含库搜索路径
LINK_DIRECTORIES(${LIB_PATH} ./lib)
LINK_DIRECTORIES(${LIB_PATH} /home/xiangang/xiangang/ffmpeg/ffmpeglib/lib)


#****************************************************************************

#3.head file path，头文件目录
INCLUDE_DIRECTORIES(
${PROJECT_SOURCE_DIR}/include
/usr/local/arm-opencv/include
/usr/local/arm-opencv/include/opencv
/usr/local/arm-opencv/include/opencv2
)

set(OpenCV_DIR /home/xiangang/xiangang/opencv/arm-opencv-v4l2)
find_package(OpenCV REQUIRED)  

message(STATUS "    OpenCV library status:")
message(STATUS "    version: ${OpenCV_VERSION}")
message(STATUS "    libraries: ${OpenCV_LIBS}")
message(STATUS "    include path: ${OpenCV_INCLUDE_DIRS}")

#****************************************************************************  
                                                                                  
# 链接动态库
SET(LIBS_LIST libqsImgLib.so libipu.so libvpu.so libavformat.so libswresample.so libavutil.so libpostproc.so)
# 添加源文件路径下所有源文件存放到变量中(*.c && *.cpp)
AUX_SOURCE_DIRECTORY(${SOURCE_PATH} SRC_LIST)
SET(EXECUTABLE_OUTPUT_PATH ${PROJECT_BINARY_DIR}/bin)


# 设置生成可执行文件的名称
SET(EXECUTABLE_FILE_NAME IMX6Q_VPU_Capture_Module.out)

# 生成可执行文件
ADD_EXECUTABLE(${EXECUTABLE_FILE_NAME} ${SRC_LIST})

# 设置可执行文件链接的库名称
TARGET_LINK_LIBRARIES(${EXECUTABLE_FILE_NAME} ${LIBS_LIST} ${OpenCV_LIBS} -lm -lpthread -lrt -ldl )
