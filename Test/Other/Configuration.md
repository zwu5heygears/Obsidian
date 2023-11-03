# <font color = #8B6CEF>VSCode</font>
c_cpp_propertites.json
```cpp
{
    "configurations": [
        {
            "name": "Win32",
            "includePath": [
                "${workspaceFolder}/**",
                "D:/Microsoft VS Code/VsCodeProjects/include/OpenCV-MinGW/include/opencv2",
                "D:/Microsoft VS Code/VsCodeProjects/include/OpenCV-MinGW/include",
                "D:/MAMP/bin/mysql/include"
            ],
            "defines": [
                "_DEBUG",
                "UNICODE",
                "_UNICODE"
            ],
            // 编译器路径
            "windowsSdkVersion": "10.0.19041.0",
            "compilerPath": "D:/Microsoft VS Code/mingw64/bin/g++.exe",
            "cStandard": "c11",
            "cppStandard": "c++11",
            "intelliSenseMode": "gcc-x64"
        }
    ],
    "version": 4
}
```

task.json
```cpp
{
    "version": "2.0.0",
    "tasks": [
        {
            "type": "cppbuild",
            "label": "C/C++: g++.exe 生成活动文件",
            "command": "D:/Microsoft VS Code/mingw64/bin/g++.exe",
            "args": [
                "-fdiagnostics-color=always",
                "-g",
                "-std=c++11",
                "${file}",
                "-o",
                "${fileDirname}\\bin\\${fileBasenameNoExtension}.exe",
                "-I",
                "D:/Microsoft VS Code/VsCodeProjects/include/OpenCV-MinGW/include",
                "-I",
                "D:/Microsoft VS Code/VsCodeProjects/include/OpenCV-MinGW/include/opencv2",
                "-L",
                "D:/Microsoft VS Code/VsCodeProjects/include/OpenCV-MinGW/x64/mingw/bin",
                "-I",
                "D:/MAMP/bin/mysql/include/",
                "-L",
                "D:/MAMP/bin/mysql/lib",
                "-l",
                "libmysql",
                "-l",
                "mysqlclient",
                "-l",
                "mysqlserver",
                "-l",
                "mysqlservices",
                "-l",
                "libmysqld",
                "-l",
                "libopencv_calib3d452",
                "-l",
                "libopencv_core452",
                "-l",
                "libopencv_dnn452",
                "-l",
                "libopencv_features2d452",
                "-l",
                "libopencv_flann452",
                "-l",
                "libopencv_gapi452",
                "-l",
                "libopencv_highgui452",
                "-l",
                "libopencv_imgcodecs452",
                "-l",
                "libopencv_imgproc452",
                "-l",
                "libopencv_ml452",
                "-l",
                "libopencv_objdetect452",
                "-l",
                "libopencv_photo452",
                "-l",
                "libopencv_stitching452",
                "-l",
                "libopencv_video452",
                "-l",
                "libopencv_videoio452",

            ],

            "options": {

                "cwd": "D:/Microsoft VS Code/mingw64/bin"

            },

            "problemMatcher": [

                "$gcc"

            ],

            "group": {

                "kind": "build",

                "isDefault": true

            },
            "detail": "编译器: D:/Microsoft VS Code/mingw64/bin/g++.exe"
        }
    ]
}
```
gcc(mingw)+opencv[URL](https://blog.csdn.net/C_say_easy_to_me/article/details/123582066)

---
# Error1
Unable to start debugging. Unexpected GDB output from command "-exec-run". During startup program exited with code 0xc0000139
> python annaconda 环境和OpenCV环境冲突 切换至Powershell不激活annaconda

![[autoCalib.canvas|AutoCalib]]
