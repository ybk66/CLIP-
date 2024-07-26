ubuntu安装opencv c++在vscode远程连接可用

1.opencv下载4.10.0，sources版本



2.vscode安装cmake tools插件



3.终端

退出anaconda虚拟环境

- conda deactivate

- libgtk2.0-dev比较重要

```
sudo apt install -y g++ make wget unzip
sudo apt install -y libssl-dev build-essential
sudo apt install -y pkg-config
sudo apt-get install build-essential libgtk2.0-dev libavcodec-dev libavformat-dev libjpeg.dev libtiff5.dev libswscale-dev libjasper-dev

sudo apt-get install cmake git libgtk2.0-dev pkg-config libavcodec-dev libavformat-dev libswscale-dev
sudo apt-get install python-dev python-numpy libtbb2 libtbb-dev libjpeg-dev libpng-dev libtiff5-dev libdc1394-22-dev
sudo apt-get install libavcodec-dev libavformat-dev libswscale-dev libv4l-dev liblapacke-dev
sudo apt-get install libxvidcore-dev libx264-dev
sudo apt-get install libatlas-base-dev gfortran
sudo apt-get install ffmpeg
```



4.opencv

在ubuntu下解压

cd /目录/opencv4.10.0

```
mkdir -p build  #-p是指若不存在必要的父目录，则创建

cd build

cmake -DCMAKE_BUILD_TYPE=Release \
-DOPENCV_GENERATE_PKGCONFIG=ON \
-DCMAKE_INSTALL_PREFIX=/usr/local ..

sudo make -j 16

sudo make install
```



5.配置

```
sudo gedit /etc/ld.so.conf.d/opencv.conf

# 写入/usr/local/lib
```

```
sudo ldconfig

sudo gedit /etc/bash.bashrc 

#写入PKG_CONFIG_PATH=$PKG_CONFIG_PATH:/usr/local/lib/pkgconfig export PKG_CONFIG_PATH
```

```
source /etc/bash.bashrc

pkg-config --modversion opencv4


```

6.进入项目文件夹

- 命令形式运行

新建CMakeLists.txt

编辑示例

```
cmake_minimum_required(VERSION 3.12)
project(try)

set(CMAKE_CXX_STANDARD 11)

find_package(OpenCV REQUIRED)

include_directories(${OpenCV_INCLUDE_DIRS})
set(CMAKE_CXX_STANDARD 11)

add_executable(test test.cpp)

target_link_libraries(test ${OpenCV_LIBS})

```

新建buid文件夹并进入

```
mkdir build
cd build
cmake ..
make

#执行可执行文件
./test
```



- vscode调试运行

创建.vscode/c_cpp_properties.json

```
{
    "configurations": [
        {
            "name": "Linux",
            "includePath": [
                "${workspaceFolder}/**",
            
                "/usr/local/include/opencv4" //此处为opencv的路径
            ],
            "defines": [],
            "compilerPath": "/usr/bin/gcc",
            "cStandard": "gnu11",
            "cppStandard": "gnu++14",
            "intelliSenseMode": "linux-gcc-x64"
        }
    ],
    "version": 4
}
```

创建.vscode/launch.json

```
{
    // Use IntelliSense to learn about possible attributes.
    // Hover to view descriptions of existing attributes.
    // For more information, visit: https://go.microsoft.com/fwlink/?linkid=830387
    "version": "0.2.0",
    "configurations": [
        {
            "name": "g++ - Build and debug active file",
            "type": "cppdbg",
            "request": "launch",
            "program": "${fileDirname}/${fileBasenameNoExtension}",  //程序文件路径
            "args": [],  //程序运行需传入的参数
            "stopAtEntry": false,
            "cwd": "${fileDirname}",
            "environment": [],
            "externalConsole": true,   //运行时是否显示控制台窗口
            "MIMode": "gdb",
            "setupCommands": [
                {
                    "description": "Enable pretty-printing for gdb",
                    "text": "-enable-pretty-printing",
                    "ignoreFailures": true
                }
            ],
            "preLaunchTask": "C/C++: g++ build active file",
            "miDebuggerPath": "/usr/bin/gdb"
        }
    ]
}
```

创建.vscode/tasks.json

```
{
    "tasks": [
        {
            "type": "cppbuild",
            "label": "C/C++: g++-9 生成活动文件",
            "command": "/usr/bin/g++-9",
            "args": [
                "-fdiagnostics-color=always",
                "-g",
                //"${file}",//编译单个文件
                "${fileDirname}/*.cpp",  //编译多个文件
                "-o",
                "${fileDirname}/${fileBasenameNoExtension}",//输出文件路径
                
                /* 项目所需的头文件路径 */
                "-I","${workspaceFolder}/",
                "-I","/usr/local/include/",
                "-I","/usr/local/include/opencv4/",
                "-I","/usr/local/include/opencv4/opencv2",
 
                /* 项目所需的库文件路径 */
                "-L", "/usr/local/lib",
 
                /* OpenCV的lib库 */
                "/usr/local/lib/libopencv_*",

            ],
            "options": {
                "cwd": "${fileDirname}"
            },
            "problemMatcher": [
                "$gcc"
            ],
            "group": {
                "kind": "build",
                "isDefault": true
            },
            "detail": "调试器生成的任务。"
        }
    ],
    "version": "2.0.0"
}
```

