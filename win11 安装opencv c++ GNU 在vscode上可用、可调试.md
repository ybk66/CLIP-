# win11 安装opencv c++ GNU 在vscode上可用、可调试

## 1.cmake3.30.1

安装windowx86-64.msi版本

![image-20240726103607372](C:\Users\75954\AppData\Roaming\Typora\typora-user-images\image-20240726103607372.png)

安装

添加桌面图标

路径改为：E:\CMake



## 2.搜：gnutoolchains--Mingw64

![image-20240726104030806](E:\Desktop\assets\image-20240726104030806.png)

mingw64-gcc12.2.0.exe

![image-20240726104222606](E:\Desktop\assets\image-20240726104222606.png)

安装

路径改为：E:\SysGCC

接受



## 3.opencv-4.10.0

![image-20240726104326030](E:\Desktop\assets\image-20240726104326030.png)

解压

路径改为：E:\



## 4.环境变量--path

![image-20240726105015954](E:\Desktop\assets\image-20240726105015954.png)

如果

gcc --version

查看是比较新的版本，我这里是12.2.0

## 5.打开cmake-gui

source code：E:/opencv/sources

build binaries：E:/opencv/build

![image-20240726110227322](E:\Desktop\assets\image-20240726110227322.png)

configure

![image-20240726110356214](E:\Desktop\assets\image-20240726110356214.png)

失败则重启windows重试

成功则点genarate



## 6.网盘下载generate.exe

复制到E:\opencv\build\bin下



## 7.在opencv/build下

mingw32-make

![image-20240726114300977](E:\Desktop\assets\image-20240726114300977.png)

## 8.有可能要重装powershell和重启，酌情做

## 9.终端-generate

generate.exe E:/opencv

![image-20240726115015814](E:\Desktop\assets\image-20240726115015814.png)



## 10.打开项目

把刚刚指示的路径都复制进去，也可以直接复制这里的整个文件

**常规方法**

c++文件，点击运行

![image-20240726115338717](E:\Desktop\assets\image-20240726115338717.png)

失败，并出现一个.vscode/tasks.json

将

```
"-IE:\\opencv\\build\\include","-LE:\\opencv\\build\\bin","-lopencv_calib3d4100", "-lopencv_core4100", "-lopencv_dnn4100", "-lopencv_features2d4100", "-lopencv_flann4100", "-lopencv_gapi4100", "-lopencv_highgui4100", "-lopencv_imgcodecs4100", "-lopencv_imgproc4100", "-lopencv_ml4100", "-lopencv_objdetect4100", "-lopencv_photo4100", "-lopencv_stitching4100", "-lopencv_video4100", "-lopencv_videoio4100", "-lopencv_videoio_ffmpeg4100_64",
```

复制到"${file}",的下一行

选中并格式化该行内容



shift+ctrl+p召唤c/c++

选编辑配置

![image-20240726120249318](E:\Desktop\assets\image-20240726120249318.png)

编译器路径：

E:/SysGCC/bin/g++.exe

IntelliSense 模式：

${default}

包含路径：加一个

E:/opencv/build/include/**

---

**直接复制**

.vscode/c_cpp_properties.json

```
{
    "configurations": [
        {
            "name": "Win32",
            "includePath": [
                "${workspaceFolder}/**",
                "E:/opencv/build/include/**"
            ],
            "defines": [
                "_DEBUG",
                "UNICODE",
                "_UNICODE"
            ],
            "windowsSdkVersion": "10.0.22621.0",
            "compilerPath": "E:/SysGCC/bin/g++.exe",
            "cStandard": "c17",
            "cppStandard": "c++17",
            "intelliSenseMode": "${default}"
        }
    ],
    "version": 4
}
```

.vscode/tasks.json

```
{
    "tasks": [
        {
            "type": "cppbuild",
            "label": "C/C++: g++.exe 生成活动文件",
            "command": "E:/SysGCC/bin/g++.exe",
            "args": [
                "-fdiagnostics-color=always",
                "-g",
                "${file}",
                "-Ie:\\opencv\\build\\include",
                "-Le:\\opencv\\build\\bin",
                "-lopencv_calib3d4100",
                "-lopencv_core4100",
                "-lopencv_dnn4100",
                "-lopencv_features2d4100",
                "-lopencv_flann4100",
                "-lopencv_gapi4100",
                "-lopencv_highgui4100",
                "-lopencv_imgcodecs4100",
                "-lopencv_imgproc4100",
                "-lopencv_ml4100",
                "-lopencv_objdetect4100",
                "-lopencv_photo4100",
                "-lopencv_stitching4100",
                "-lopencv_video4100",
                "-lopencv_videoio4100",
                "-lopencv_videoio_ffmpeg4100_64",
                "-o",
                "${fileDirname}\\${fileBasenameNoExtension}.exe"
            ],
            "options": {
                "cwd": "E:/SysGCC/bin"
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

