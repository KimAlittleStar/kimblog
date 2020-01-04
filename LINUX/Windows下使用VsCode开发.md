# 使用VsCode+makefile开发C/C++

**1. 介绍**

　　vscode作为现在越来越受欢迎的编辑器之一，因为可以使用插件让vscode支持几乎市面上所有的编程语言，由于笔者主要接触的是 C/C++ 方面，因此在这里简单介绍一下如何搭建vscode编译、调试C/C++项目的过程；整套环境完全使用开源软件进行搭建，只需要做很少的改变就可以无缝搬移到linux中；采用的方案是：vscode+git+mingw gcc+makefile；最后有详细的技术说明和资源分享（GitHub 项目：[vscode_c_demo](https://github.com/KimAlittleStar/vscode_c_demo)）；

　　介绍一下笔者搭建的平台和所需要的软件:

![Windows](https://upload-images.jianshu.io/upload_images/19845544-2e797a2208003840.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)    
![VsCode](https://upload-images.jianshu.io/upload_images/19845544-4d71695c1d6984f8.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)    
![Git](https://upload-images.jianshu.io/upload_images/19845544-b965e7d31c351c45.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)    
![MinGW](https://upload-images.jianshu.io/upload_images/19845544-a9bb124df28acc68.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)    
![C/C++插件](https://upload-images.jianshu.io/upload_images/19845544-013c1a032b906f8e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)    
![简体中文包](https://upload-images.jianshu.io/upload_images/19845544-c601d3fa09dbbced.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)  

1. win7 以上；

2. VS Code [官方下载地址](https://code.visualstudio.com/)；

3. Git ([官方下载地址](https://git-scm.com/downloads))；

4. mingw GUN 编译工具 ([官方下载地址](https://osdn.net/projects/mingw/releases/))；

5. 在VS Code中你需要安装

    5.1. C/C++ 插件（用于智能代码提示）；

    5.2. Chinese (Simplified) Language Pack（中文简体支持包，英文好的请忽略）

下载安装VsCode插件比较简单；点击右侧图标搜索即可；![插件
](https://upload-images.jianshu.io/upload_images/19845544-3c6940d6d5717b6d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240) ，或者 快捷键 "Ctrl+Shift+X",搜索上述插件名称即可；

笔者不建议在mingw 的官网下载GCC 编译器；因为网络的速度会很慢，会等很久；很多开源的编译器都会自带mingwGcc编译器；例如DEVC++ 、Qt 等；

**这里提供一个百度链接 [https://pan.baidu.com/s/1x-jXHtiiWI0Kc1l34M_LrA](https://pan.baidu.com/s/1x-jXHtiiWI0Kc1l34M_LrA)       提取码：8q0d  ；里面有上述所需要的所有安装包；截至2019年11月8日，里面的安装包均是最新版本，里面请区分32bit和64bit；**

** 2.安装软件**

上述所有软件下载好之后，我们就可以开始安装程序了，所有程序可以一起安装，这里我们只讲解安装过程中的注意事项；

首先安装中，安装路径中**不能出现中文；尽量避免空格 ！**

---------------------------------------------------------------------------------------华丽丽分割线---------------------------------------------------------------------------------------

**2.1  mingw GUN GCC 编译器**

mingw 在我的百度云链接中是压缩包，我们把他解压到指定位置后（我解压的位置是C:/mingw32 ），将编译器的bin目录添加到系统环境变量；如何添加系统环境变量请自行百度；

查看是否添加成功：打开 cmd（window + R 输入cmd打开命令行窗口） 输入  ** g++ --version **  如果出现类似以下字样表示设置环境变量成功；

![查看GCC版本](https://upload-images.jianshu.io/upload_images/19845544-af48cf53dd812567.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

 ---------------------------------------------------------------------------------------华丽丽分割线---------------------------------------------------------------------------------------

**2.2  Git**

git 的默认编辑器是 vim 如果你从来没有听说过Git，那么请一路 next 安装；如果你有听过git ；我想我这个菜鸟也帮不了你啥~；如果你正在学习git，你可以看看这篇文章：[git学习笔记--基础运用](https://www.cnblogs.com/kimalittlestar/p/11598564.html)

  ---------------------------------------------------------------------------------------华丽丽分割线---------------------------------------------------------------------------------------

**2.3  VS Code**

安装vscode 也比较简单，只有一个需要注意

![安装VsCode](https://upload-images.jianshu.io/upload_images/19845544-c7dd520e77f272db.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)  

一定要将 “通过Code 打开操作” 添加到目录的上下文菜单，后期作用会比较大；其他选项请自行选择；

 ---------------------------------------------------------------------------------------华丽丽分割线---------------------------------------------------------------------------------------

**2.4  安装插件**
![安装C语言插件](https://upload-images.jianshu.io/upload_images/19845544-31b43194e03850fb.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
![安装中文包插件](https://upload-images.jianshu.io/upload_images/19845544-09e3e7f0a3233241.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

插件安装之后会提示重启vscode 激活插件，重启即可

 ---------------------------------------------------------------------------------------华丽丽分割线---------------------------------------------------------------------------------------

**3.配置参数**

** 1. 导入一个示例项目**

这边你们可以随便导入一个项目，可以有多个文件夹，文件夹中可以有.c/.h文件，但是有一个要求，就是在里面的文件不存在冲突；即文件夹里面的源文件都是需要用到的，就算没有用到，也没有命名冲突等系列问题；

因为接下来的这个makefile会自动将你项目文件夹下所有的.c/.h文件全部编译，如果出现冲突，那么请你自己解决；最好的情况是：你用到了哪些文件你就放入哪些文件，不需要的文件可以修改他的后缀名；

为了方便展示，我从github 上clone 一个项目到文件夹下；在文件夹空白处右键单击，选择git bash here 输入：

```bash
git clone https://github.com/KimAlittleStar/vscode_c_demo.git
```

克隆成功后，右键点击生成出来的 vscode_c_demo文件夹；open with vscode ；文件树如下：

![项目文件树](https://upload-images.jianshu.io/upload_images/19845544-ec61eb64b2ae60b8.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

 ---------------------------------------------------------------------------------------华丽丽分割线---------------------------------------------------------------------------------------

**2.设置默认的shell 命令行**

点击左下角 齿轮图标 选择 设置 或者  点击 文件->首选项->设置 打开如下界面

**如果你选择用户,那么更改的是默认配置,如果你选择工作区,那么你做的设置只会在这个文件夹内部起作用;**

![修改shell](https://upload-images.jianshu.io/upload_images/19845544-6679b9f76b667478.gif?imageMogr2/auto-orient/strip)

  ---------------------------------------------------------------------------------------华丽丽分割线---------------------------------------------------------------------------------------

**3.让VScode 提供智能提示**

![修改编译器默认路径](https://upload-images.jianshu.io/upload_images/19845544-7bb16538895be955.gif?imageMogr2/auto-orient/strip)


```json
//C:/Git/bin/bash.exe 应该是你安装的git的bin目录下的bash.exe//C:/mingw32/bin/gcc.exe 应该是你安装的mingw下bin目录的gcc.exe//如果你的项目是C++ ,请换成 g++.exe//如果复制此段单吗到你的 setting.json文件后出错,请删除注释{    "terminal.integrated.automationShell.windows":"C:/Git/bin/bash.exe",    "C_Cpp.default.compilerPath": "C:/mingw32/bin/gcc.exe"}
```


  ---------------------------------------------------------------------------------------华丽丽分割线---------------------------------------------------------------------------------------

**4.设置编译任务task.json 和 运行设置  launch.json**



```json
{ // See https://go.microsoft.com/fwlink/?LinkId=733558
    // for the documentation about the tasks.json format    //此 json 文件中需要注意的就是 执行makefile的执行文件是 mingw32-make.exe ,如果你使用qmake 或者其他make 执行文件,替换它即可;其他不需要修改
    "version": "2.0.0",
    "tasks": [
        {
            "label": "build", //task的名字,调用方式就是 task build  
            "command": "mingw32-make.exe", //会在命令行中调用此命令  
            "args": [ //调用上述 mingw32-make.exe 传递给它的参数  
                "target=${workspaceRootFolderName}.exe" //${workspaceRootFolderName} 会被替换成 根目录 即:vscode_c_demo            
            ],
            "type": "shell",
            "problemMatcher": []
        }, //此命令等效展开:mingw32-make.exe target=vscode_c_demo.exe        
        {
            "label": "build-debug",
            "command": "mingw32-make.exe",
            "args": [
                "target=${workspaceRootFolderName}.exe",
                "DEBUG=-g", //添加debug 参数 使gcc 生成调试信息
                "PREDEF=Debug" //相当于在程序中定义了一个宏定义#define Debug            
            ],
            "type": "shell"
        }, //此命令等效展开为:mingw32-make.exe target=vscode_c_demo.exe DEBUG=-g PREDEF=Debug        
        {
            "label": "clean",
            "command": "mingw32-make.exe",
            "args": [
                "clean",
                "target=${workspaceRootFolderName}.exe"
            ],
            "type": "shell",
            "problemMatcher": []
        }, //此命令等效展开为:mingw32-make.exe clean target=vscode_c_demo.exe        
        {
            "label": "runing",
            "command": "./runExcute.sh",
            "args": [
                "${workspaceRootFolderName}.exe" // 传给脚本的参数            
            ],
            "type": "shell"
        } //此命令等效展开为:mingw32-make.exe target=vscode_c_demo.exe  然后执行 ./vscode_c_demo.exe    
    ]
}
```




```json
{
    // 使用 IntelliSense 了解相关属性。 
    // 悬停以查看现有属性的描述。
    // 欲了解更多信息，请访问: https://go.microsoft.com/fwlink/?linkid=830387
    // 
    //这里我们需要注意修改的是:"miDebuggerPath": "C:/Qt/Tools/mingw730_64/bin/gdb.exe"
    //
    //这里gdb的路径应该是你自己的gdb 文件路径
    // 
    // "program": "${workspaceFolder}/${workspaceRootFolderName}.exe"
    // 这里调用的exe 应就是 task build-debug 中生成的可执行文件;
    "version": "0.2.0",
    "configurations": [
        {
            "name": "(gdb) 启动", //名字,会显示在debug 的选项中
            "type": "cppdbg",
            "request": "launch",
            "program": "${workspaceFolder}/${workspaceRootFolderName}.exe", //表示要调试的可执行文件在当前打开的文件下下的这个文件,此名称需要和task 中的target=<>,一一对应;
            "args": [],
            "stopAtEntry": false, //时候要在main函数处自动停止,false表示不会停止,true表示会在main函数处自动停止;
            "cwd": "${workspaceFolder}", //表示首先要切换到当前目录下
            "environment": [],
            "externalConsole": false, //为true时,会新建一个黑窗口运行程序,如果为false ,就会在vscode中新建终端,
            //不过这样就需要在用户设置中设置默认bash 为 gitbash,否则会报错
            "MIMode": "gdb",
            "miDebuggerPath": "E:/userFile/userSoftware/Qt/Tools/mingw730_64/bin/gdb.exe", // 这里填写的应该是你自己gdb.exe文件的路径;一般与g++.exe minwg32-make.exe 在同一文件夹下
            "setupCommands": [
                {
                    "description": "为 gdb 启用整齐打印",
                    "text": "-enable-pretty-printing",
                    "ignoreFailures": true
                }
            ],
            "preLaunchTask": "build-debug" //在执行这个gdb launcher 之前,首先执行task build-debug;
        }
    ]
}
```


![调用 task](https://upload-images.jianshu.io/upload_images/19845544-b5c582727659f0ac.gif?imageMogr2/auto-orient/strip)

贴一下task.josn 和 launch.json 的代码

  ---------------------------------------------------------------------------------------华丽丽分割线---------------------------------------------------------------------------------------

**5.尝试运行**

快捷键  CTRL + P 打开命令行 输入 task+空格+任务命令 即可执行相应命令

task.json中一共创建了4个命令； 分别是  build build-debug clean running



```bash
task build #创建一个文件夹名字相同的可执行文件；并且所有的中间文件都会生成放在build文件夹下；

task build-debug #与build类似，但是会生成调试信息，如果你想使用gdb等工具调试，那么必须使用此选项；

task clean #清除所有由build带来的文件

task runing #一共分两步，首先build ，其次调用其可执行文件；
```


![运行](https://upload-images.jianshu.io/upload_images/19845544-a1d72e60211abc6d.gif?imageMogr2/auto-orient/strip)

 ---------------------------------------------------------------------------------------华丽丽分割线---------------------------------------------------------------------------------------

** 6 DEBUG 选项**

vscode Debug使用的是mingw32中的gdb.exe调试，与平常的断点调试并无太大差异；

具体使用方法如下:点击左侧导航键 虫子的图标，选择（gdb）启动，点击右方绿色的开始图标，vscode会自动开始编译，运行到断点处会自动停止，更多设置请参考上文launch.json中的注释

![debug单步调试](https://upload-images.jianshu.io/upload_images/19845544-43dd05f1bd3390e5.gif?imageMogr2/auto-orient/strip)

  ---------------------------------------------------------------------------------------华丽丽分割线---------------------------------------------------------------------------------------

**4.原理分析**  

        整个环境搭建最主要的工作在于：**mingw32-make.exe** 中，实际上，如果你安装了git ，并按照上述方式修改了默认的终端为 gitbash，那么你只需要在终端中输入 mingw32-make.exe target=a.exe ,也会自动生成一个a.exe的可执行文件，在终端输入mingw32-make.exe clean target=a.exe，也可以清除所有由build带来的所有新建的中间依赖文件；task.json的作用就在于 当我 输入 task build 时 ，就相当于我在gitbash中输入了mingw32-make.exe target=DirName.exe 这个命令；

　　接下来我们会分析为什么mingw32-make.exe 可以做到这些，如果你仔细观察我们的文件树，你会发现文件树中有一个没有后缀的文件叫 makefile，我们可以打开makefile 文件，



```makefile
#此项目源文件后缀类型
PROJECTTYPE = .c

#您想要生成可执行文件的名字 如果外部没有赋值,那么使用obj.out
target ?= obj.out

#是否生成DEBUG选项
DEBUG ?=#系统之外的宏定义
PREDEF ?=#获取当前makefile绝对路径
pes_parent_dir:=$(dir $(abspath $(lastword $(MAKEFILE_LIST))))////#删除路径下最后一个 / pes_parent_dir:=$(subst /////,,$(pes_parent_dir))#获得mkfile 的实际路径  测试使用,  没有实际用到 可删除
mkPath:=$(dir $(abspath $(lastword $(MAKEFILE_LIST))))////mkPath:=$(subst /////,,$(mkPath))#将所有宏定义前方加入-D指令以便给编译器识别
DEF := $(foreach n,$(PREDEF),-D$(n))

#获取目录下所有子目录
AllDirs := $(shell cd $(pes_parent_dir); ls -R | grep '^./.*:$$' | awk '{gsub(":","");print}') .

#添加成为绝对路径
AllDirs := $(foreach n,$(AllDirs),$(subst .,$(pes_parent_dir),$(n)))

#获取所有 .c/.cpp文件路径
Sources := $(foreach n,$(AllDirs) , $(wildcard $(n)/*$(PROJECTTYPE)))

#设置*.o 和 *.d 文件的存放路径
buildPath :=$(foreach n,$(Sources),$(subst $(pes_parent_dir),$(pes_parent_dir)/build,$(n)))

#处理得到*.o 后缀文件名
OBJS := $(patsubst %$(PROJECTTYPE),%.o, $(buildPath))  

#同理得到 *.d文件名
Deps := $(patsubst %$(PROJECTTYPE),%.d, $(buildPath))  

#需要用到的第三方静态库
StaticLib :=

#需要用到的第三方动态链接库
DynamicLib := 

#真实二进制文件输出路径
Bin :=$(pes_parent_dir)/$(target)

#C语言编译器
CC = gcc

#C++编译器
CXX = g++

#简化rm -f
RM = rm -f

#C语言配置参数
CFLAGS = -g  -pedantic -std=c11 -Wall -o

#C++配置参数
CXXFLAGS = -g -Wall -std=c11 

#头文件搜索路径
INCLUDE_PATH = $(foreach n,$(AllDirs) , -I$(n))
LDFLAGS = 

#指定AllLibs为终极目标 即:最新的Bin 
AllLibs:$(Bin)

#声明这个标签 des 用于观察当前的路径是否正确
.PHONY:des
des:
    @echo OBJS =  $(OBJS)
    @echo cur_makefile_path = $(pes_parent_dir)
    @echo AllDirs = $(AllDirs)
    @echo Sources = $(Sources)
    @echo Deps = $(Deps)
    @echo makefilePath =$(mkPath)
    @echo bulidPath=$(buildPath)
    @echo PREDEF = $(DEF)
    @echo DEBUG = $(DEBUG)

#对应关系 在本makefile中以空格隔开的后缀为.c 都会为其生成一个新的.d文件
#$(pes_parent_dir)/build/%.d :$(pes_parent_dir)/%.c  
#       @echo 'finding $< depending head file'
#       @if [ ! -d $(dir $@) ]; then mkdir -p $(dir $@); fi;
#       @$(CC) -MT"$(<:.c=.o) $@" -MM $(INCLUDE_PATH) $(CPPFLAGS) $< > $@


#对应关系 在本makefile中以空格隔开的后缀为.c 都会为其生成一个新的.d文件
$(pes_parent_dir)/build/%.d :$(pes_parent_dir)/%.c
    @echo 'finding $< depending head file'
    @if [ ! -d $(dir $@) ]; then mkdir -p $(dir $@); fi;
    @set -e; rm -f $@; 
    $(CC) -MM $(INCLUDE_PATH) $(DEF) $(CPPFLAGS) $< > $@.$$$$; 
    sed 's,($*).o[ :]*,1.o $@ : ,g' < $@.$$$$ > $@; 
    rm -f $@.$$$$


#对于include中的*.d文件，只要里面任意有一个文件被修改，那么就会触发此规则生成一个新的*.o文件
%.o: %.d
    @echo compile $(patsubst %.d,%.c,$(subst build/,,$<)) 
    @$(CC) -c $(patsubst %.d,%.c,$(subst build/,,$<)) $(DEBUG) $(DEF) $(INCLUDE_PATH) $(CFLAGS) $@ 



$(Bin) : $(OBJS)  
    @echo bulding....
    @$(CC) $(OBJS)  $(CFLAGS) $(Bin)
    @echo created file: $(target)    

.PHONY : clean  
clean:   
        @echo '清理所有文件ing...'
        @$(RM) -r $(pes_parent_dir)/build/
        @echo '清理可执行文件ing...'
        @$(RM) $(Bin)
        @echo 'done'

.PHONY : cleanO
cleanO:
        @echo '清理Obj && Dep'
        @$(RM) -r $(pes_parent_dir)/build
        @echo 'done'
#main.out: $(OBJ)
#    cc -o main.out $(OBJ)

include $(buildPath:.c=.d)
```


 　　在上述makefile 中，比较关键的语句在于


```makefile
= $(shell cd $(pes_parent_dir);  -R |   |  ) .
···
#对应关系 在本makefile中以空格隔开的后缀为.c 都会为其生成一个新的.d文件
$(pes_parent_dir)/build/%.d :$(pes_parent_dir)/%.c
@echo 'finding $< depending head file'
@if [ ! -d $(dir $@) ]; then mkdir -p $(dir $@); fi;
@set -e; rm -f $@; 
$(CC) -MM $(INCLUDE_PATH) $(DEF) $(CPPFLAGS) $< > $@.$$$$; 
sed 's,($*).o[ :]*,1.o $@ : ,g' < $@.$$$$ > $@; 
rm -f $@.$$$$
```


 `shell cd  $(pes_parent_dir)` 表示切换路径到该makefile 的目录下，然后 `ls -R `：递归输入该目录下的所有文件以及文件夹；如果是文件夹，那么就会单独起一行然后文件夹名字前有 ‘.’ 作为标记 后面的`grep` 和 `awk` 、`gsup` 都是linux下才有的命令；

同理在 下方` if [ ! -d $(dir $@) ]; then mkdir -p $(dir $@); fi; `所做的逻辑就是判断当前想要生成的*`.d文件`的目标文件夹在不在， 不在则创建它， 其中`dir`  `mkdir` 都是linux中才会有的命令，当然`windows`下也有与其差不多功能的命令，但是此makefile我希望在linux下也能用，这个时候怎么办呢，这个时候就要介绍我们的 Git 了，git在windows中使用的命令行软件 bash.exe 就把上面的命令都覆盖到了，也就是说，如果我们使用windows 自带的cmd.exe 或者 powerShell.exe 里面都会使得 `dir` `mkdir` 和其他命令找不到，导致makefile错误终止；所以我们才**需要安装Git** ，所以我们才需要将**Git bash 设置成为vsCode 默认的终端**；因为**只有git bash 中才可以调用上述makefile中的几个shell命令**；

还记的我们将`mingw32编译器`的路径加入到了系统环境变量里面去了~为什么呢？

```makefile
#C语言编译器
CC = gcc#C++编译器
CXX = g++
```

因为 在makefile 的编译器中，`gcc` 、`g++` 只是简单的字符，只有将gcc 、g++ 的路径加入了环境变量，他们才能正确的被找到；

还有，如果我们想管理`C++ `项目这个时候怎么办呢~我们只需要在 makefile中将所有的 `$(CC)` 替换成 `$(CXX)`  所有 的`$(CFLAG)` 替换成 `$(CXXFLAG)` 就可以了哦~

makefile中其他的只是我就不赘述了，你可以参考这个链接：[一个自动管理项目的makefile](https://www.cnblogs.com/kimalittlestar/p/11540683.html)

最后献上我整个项目的**github地址**：**[https://github.com/KimAlittleStar/vscode_c_demo](https://github.com/KimAlittleStar/vscode_c_demo)**

里面有所有的项目文件，以及配置文件，和较为详细的注释；只需要修改几个简单的路径就可以完成上述的配置；我都有在json文件里面说明哦~~~

 ---------------------------------------------------------------------------------------华丽丽分割线---------------------------------------------------------------------------------------

如果关注度还可以的话;

我会选择更新:

**一键/自动转换 C/C++ 项目的makefile,**

**编写一个只清楚中间文件,而不清除可执行文件的task**

**搭建VSCode + 嵌入式 ARM编译调试环境**

**码字不易,觉得稍微有点用的,可以在git里面 star一下,博客点个小赞;**

**读书的时候就想了2、3年的事情,现在终于快要看到成功了;**