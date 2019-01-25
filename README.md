**VS code常规C工程建立方法
  
# github设定
··· shell
git init  
git add .  
git remote add origin https://github.com/bigdot123456/VSCode_C_Demo.git  
git push origin master  
```

# 环境配置


# VSCode_C_Demo
use as a startup C++ configure
在你的计算机中选择一个合适的位置，作为你的 C 语言开发工作区。建议工作区所在路径仅由字母、数字、下划线组成，不要包含其他的符号。例如空格，空格符号常常作为命令行中命令和参数的间隔符，如果路径包含空格会导致编译时出错。我创建的工作区的路径为： D:\Study\C\WorkSpace

由于 Windows 中文版命令行输出字符是 GB2312 编码的，而 VS Code 工作区默认是 UTF-8 ，这会导致你编写的 C 代码编译后在命令行执行并查看结果时中文会显示乱码，所以我们要单独针对工作区进行设置字符编码，保证程序输出的字符也采用跟命令行一致的 GB2312 编码，步骤如下：

使用 VS Code 打开你创建的工作区；
在 VS Code 左下角的设置按钮进设置，再点击 用户设置 旁边的 工作区设置 ；
在 工作区设置 中添加 "files.encoding":"gb2312"

5. 编写你的第一个 C 语言程序
在工作区新建一个 C 语言源文件命名为 hello.c ，输入以下内容：

#include <stdio.h>
#include <windows.h>
int main()
{
    printf("hello world!/n");
    system("pause");
}
6. 配置导入的头文件参数 ccppproperties.json
在编写完毕并保存之后，你可能会看到 #include 这句下面会有绿色波浪线，这是由于编译器没办法找到你所使用的头文件的所在位置。将光标移动到该行，行号左边会出现 黄色小灯泡 ，点击会出现一个提示按钮： Addinclude path to setting ，继续点击该提示，则会在工作区 .vscode 下生成 c_cpp_properties.json 文件。将文件修改成下面内容：

{
    "configurations": [
        {
            "name": "Win32",
            "intelliSenseMode": "msvc-x64",
            "includePath": [
                "${workspaceFolder}",
                // 下面路径中的 D:/App/MinGw 部分需要替换成你的 MinGw-w64 安装路径
                "D:/App/MinGW/mingw64/include",
                "D:/App/MinGW/mingw64/opt/include",
                "D:/App/MinGW/mingw64/opt/lib/libffi-3.2.1/include",
                "D:/App/MinGW/mingw64/x86_64-w64-mingw32/include",
                "D:/App/MinGW/mingw64/lib/gcc/x86_64-w64-mingw32/7.3.0/include",
                "D:/App/MinGW/mingw64/lib/gcc/x86_64-w64-mingw32/7.3.0/include-fixed",
                "D:/App/MinGW/mingw64/lib/gcc/x86_64-w64-mingw32/7.3.0/install-tools/include"
            ],
            "defines": [
                "_DEBUG",
                "UNICODE",
                "__GNUC__=7",
                "__cdecl=__attribute__((__cdecl__))"
            ],
            "browse": {
                "path": [
                    "${workspaceFolder}",
                    // 下面路径中的 D:/App/MinGw 部分需要替换成你的 MinGw-w64 安装路径
                    "D:/App/MinGW/mingw64/include",
                    "D:/App/MinGW/mingw64/opt/include",
                    "D:/App/MinGW/mingw64/opt/lib/libffi-3.2.1/include",
                    "D:/App/MinGW/mingw64/x86_64-w64-mingw32/include",
                    "D:/App/MinGW/mingw64/lib/gcc/x86_64-w64-mingw32/7.3.0/include",
                    "D:/App/MinGW/mingw64/lib/gcc/x86_64-w64-mingw32/7.3.0/include-fixed",
                    "D:/App/MinGW/mingw64/lib/gcc/x86_64-w64-mingw32/7.3.0/install-tools/include"
                ],
                "limitSymbolsToIncludedHeaders": true,
                "databaseFilename": ""
            },
            "cStandard": "c11",
            "cppStandard": "c++17"
        }
    ],
    "version": 4
}
留意带注释部分的内容，需要将路径修改成你自己的安装路径哦。

7. 配置调试程序 launch.json
打开已经编写好的 hello.c ，然后按 F5 调试。因为是第一次调试，系统会弹出 选择环境 面板，这里选择 C++(GDB/LLDB) 。


选择运行环境后，VS Code 会在工作区 .vscode 文件夹下创建 luanch.json 模板文件并打开，将文件内容清空，复制下面的内容到文件中并保存：

{
    "version": "0.2.0", //配置版本
    "configurations": [
        {
            // 配置名称，在启动配置下拉菜单中显示
            "name": "(gdb) Launch", 
            // 调试会话开始前要运行的任务
            "preLaunchTask": "build", 
            // 配置类型
            "type": "cppdbg", 
            // 请求配置类型，可以为启动（launch）或附加（attach）
            "request": "launch", 
            // 将要进行调试的程序的路径
            "program": "${fileDirname}/${fileBasenameNoExtension}.exe", 
            // 程序调试时传递给程序的命令行参数，一般设为空即可
            "args": [], 
            // 设为true时程序将暂停在程序入口处，一般设置为false
            "stopAtEntry": false, 
            // 调试程序时的工作目录，一般为${workspaceFolder}即代码所在目录
            "cwd": "${workspaceFolder}", 
            "environment": [],
            // 调试时是否显示控制台窗口，一般设置为true显示控制台
            "externalConsole": true, 
            "MIMode": "gdb",
            // GDB的路径，注意替换成自己的位置中的 gdb
            "miDebuggerPath": "D:/App/MinGW/mingw64/bin/gdb.exe", 
            "setupCommands": [
                {
                    "description": "Enable pretty-printing for gdb",
                    "text": "-enable-pretty-printing",
                    "ignoreFailures": true
                }
            ]
        }
    ]
}
留意 luanch.json 中的注释内容，记得把 "miDebuggerPath" 参数修改成你自己安装位置里的 gdb.exe

gdb.exe 位于 {MinGW-w64安装位置}\mingw64\bin 下面。

8. 配置调试前执行的任务 task.json
再按一次 F5 ，会弹出“找不到任务”的提示窗口，点击 配置任务 按钮，如下图所示：


然后在弹出的命令面板选择 使用模板创建task.json文件 ，如下图所示：


继续选择 Others运行任意外部命令的示例 ，如下图所示：


完成以上步骤之后，会在工作区的 .vscode 目录下生成 tasks.json 文件，并自动打开 task.json 文件。


接下来我们将 task.json 文件内容清空，复制下面的内容到文件中并保存：

{
    "version": "2.0.0",
    "tasks": [
        {
            "label": "build",
            "type": "shell",
            // 编译参数 ggc -g ${file} -o ${fileDirname}${fileBasenameNoExtension}.exe
            "windows": {
                "command": "gcc",
                "args": [
                    "-g",
                    "\"${file}\"",
                    "-o",
                    "\"${fileDirname}\\${fileBasenameNoExtension}.exe\""
                ]
            },
            // 控制台输出的错误信息
            "problemMatcher": {
                "owner": "cpp",
                "fileLocation": [
                    "relative",
                    "." // ${workspaceFolder}
                ],
                "pattern": {
                    "regexp": "^(.*):(\\d+):(\\d+):\\s+(warning|error):\\s+(.*)$",
                    "file": 1,
                    "line": 2,
                    "column": 3,
                    "severity": 4,
                    "message": 5
                }
            },
            "group": {
                "kind": "build",
                "isDefault": true
            },
            // 终端面板配置
            "presentation": {
                "echo": true,
                "reveal": "always",
                "focus": false,
                "panel": "shared" // 控制面板是否在任务中共享面板，shared=共享，new=新面板
            }
        }
    ]
}
9. 完成
到这里，C 开发环境就已经配置完毕。接下来我们在 hello.c 的编辑窗口按 F5 运行下，看下效果。


如果你还想再创建其他的 C 语言开发工作区，我们只需要新建一个文件夹，再把现在已有工作区目录下的 .vscode 文件夹复制到新建的文件夹即可。
