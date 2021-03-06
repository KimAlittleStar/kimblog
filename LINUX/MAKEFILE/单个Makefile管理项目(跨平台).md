# 单个MakeFile管理项目(跨平台)

-----------
### ***阅读本文,你应该要拥有基础的linux知识和基本的makefile知识***
-----------
  - ***摘要**:使用一个MakeFile就可以管理整个项目,新建项目时只需要将模板Make File移动至新项目根目录即可;Windows(MinGw),Linux(GCC)系统实测可用,理论上支持makefile的所有平台皆可使用;同时配置对应变量即可实现 C / C++ 嵌入式C 的项目管理;*
    
  - ***局限**:某一种项目只能自动导入某个单一后缀的源文件(头文件没有影响);例如C++ 只能识别到`*.cpp`的文件,对于 `*.c` 类型并不能识别;*  
 
## 1. makefile的超简单入门知识  

  * 为什么要使用makefile  
    在没有图像界面的linux控制窗口中,编译代码都是使用命令行来进行编译;例如:最为简单的C语言编译
    ```bash
    gcc hello-world.c -o hello.out #编译hello-world.c文件生成可执行文件 hello.out
    ```
    对于单个文件编译可能十分简单;但是我们真正使用C/C++的使用都不会仅仅使用一个文件,通常的多个文件夹,多个文件嵌套使用.因此这里就需要使用makefile来做一个自动化编译的脚本;
    例如我们把 上述 ``` gcc hello-world.c -o hello.out ```写进一个名字为makefile的文件中,然后使用`make`指令,就可以实现与上局完全一样的功能;除此之外还增加了许多新的令人大赞的功能;

  * makefile的执行原则
    makefile的执行原则:
    ```
    <目标文件>:<源文件>
        生成目标文件的命令行方法
    ```
    - 所有的makefile中只有这一个最为重要,其他的就像函数中被main调用的函数,你只要找到它,那么你就可以慢慢的梳理整个makefile在处理些什么;  
    - makefile是一个基于文件操作的脚本,它会根据文件的最新修改日期选择性的做是否执行 `生成目标文件的命令行方法` .这一点十分有用处,例如某大型项目一共1000多个源文件,如果我们每编译一次,都需要1000多个文件同时编译链接,那么基本上我们上班->执行编译命令->执行->debug,2到3轮之后既可以下班回家了,如果是使用makefie,那么他将会只编译你改动了的文件(为理解方便),从而达到节省时间的目的;  
    - 具体规则:  
      |            目标文件状态 |          源文件状态           | 是否执行下方命令 |
      | ----------------------: | :---------------------------: | :--------------- |
      |                  不存在 |            不存在             | 不执行 && 报错   |
      |                  不存在 |             存在              | 执行             |
      | 存在 & 最新编辑时间最晚 |             存在              | 不执行           |
      |                    存在 | 存在&任意一个文件编辑时间最晚 | 执行             |

      并且主目标文件的源文件也可以是其他目标关系的目标文件,当出现需要时会进行递归调用最终生成主目标文件;第一个出现的`[公式]`就是主目标文件;
     
  * makefile的应用场景  
    - makefile大量应用在各种项目场合,开源项目几乎都是用此方式管理和编译项目.但是超大型项目一般都是使用多个makefile穿插在各个文件夹中,这样可以很方便的管理各个模块的开关和复用.  
    - 但是对于处于学习阶段的同学来说,他可能会频繁的新建项目验证各类问题,但是说大不大,说小不小,如果使用多个makefile有点大材小用,用单个makefile是最好的选择;  
    - 但是单个makefile现在网络上大多数都是需要你自己手动加入文件路径的makefile,作为懒人的我,肯定不想这样啦,于是,就有了这边文章;
   
## 2. 单个makefile管理项目思维拓展  
  - 单个makefile管理项目,首先需要递归的寻找到当前问件夹中的所有文件
  - 依次找到每个源文件对应的需要h文件,并且建立对应关系
  - 定义编译器宏,文件后缀宏,等等为跨平台做良好接口
  - 自定义复制文件结构将所有的中间文件生成至build文件夹中并保持原有文件结构方便查看
  - 定义clean伪目标,清理中间文件
  - 定义各种伪目标清理build文件夹,清理可执行文件等

## 3. makefile源码分析  
 - 接下来我会使用基础步骤由最终的目标文件开始介绍,逐次的转向内部实现;
  ```makefile
  #指定AllLibs为终极目标 即:最新的Bin 
  AllLibs:$(Bin)
  
  #生成文件的最终表达式
  $(Bin) : $(OBJS)  
		@echo bulding....
		@$(COMPILER) $(OBJS) $(ARGS)  $(COMPILERFLAG) $(Bin)
		@echo created file: $(target)
  ```
  COMPILER:编译器的宏定义,展开后一般为gcc或g++,当然也可以自己定义
  ```makefile
  COMPILERFLAG = 
  #条件编译使得其满足C项目 以及 C++项目
  ifeq ($(TYPE),C)		#如果TYPE为"C" 那么COMPILER = gcc
		COMPILER := $(CC)
		COMPILERFLAG := $(CFLAGS)
		PROJECTTYPE = .c
	else
  ifeq ($(TYPE),C++)	#如果TYPE为"C++" 那么COMPILER = g++
		COMPILER := $(CXX)
		COMPILERFLAG := $(CXXFLAGS)
		PROJECTTYPE = .cpp
  else
  ifeq ($(TYPE),ARMC)	#如果TYPE为"ARMC",那么COMPLIER = gcc 实际上这里应该是arm-gcc交叉编译链的gcc
		COMPILER := $(CC)
		COMPILERFLAG := $(CFLAGS)
		PROJECTTYPE = .c
  else					#3个都不是,所有设置清空
		COMPILER :=
		COMPILERFLAG := 
		PROJECTTYPE := 
  endif
  endif
  endif
  ```
  - OBJS的生成与关系
  ```makefile
  #处理得到*.o 后缀文件名
  OBJS := $(patsubst %$(PROJECTTYPE),%.o, $(buildPath))  
  
  #对于include中的*.d文件，只要里面任意有一个文件被修改，那么就会触发此规则生成一个新的  *.o文件
  %.o: %.d
  	@echo compile $(patsubst %.d,%$(PROJECTTYPE),$(subst build/,,$<)) 
  	@$(COMPILER) -c $(patsubst %.d,%$(PROJECTTYPE),$(subst build/,,$<)) $  (DEBUG) $(DEF) $(INCLUDE_PATH) $(ARGS) $(COMPILERFLAG) $@ 
  
  include $(buildPath:$(PROJECTTYPE)=.d) # 这句话很重要,你可以编译之后打开.d文件你就明白啦

  ```
  - 默认的主要表达式
  ```makefile
  #生成文件的最终表达式
  $(Bin) : $(OBJS)  
    @echo bulding....
    @$(COMPILER) $(OBJS) $(ARGS)  $(COMPILERFLAG) $(Bin)
    @echo created file: $(target)	
  ```
  上面有一行
  ```makefile
  #指定AllLibs为终极目标 即:最新的Bin 
  AllLibs:$(Bin)
  ```
  也就是说 获得最新的 Bin是默认目标,依存为 Objs的诸多文件,而生成途径 就是使用之间我们生成的 OBJS 的*.o文件链接起来生成 target;


  - 其他目标任务
  ```makefile
  #删除所有编译带来的文件
.PHONY : clean  
clean:clean_out clean_build clean_bin

#删除 *.o文件
.PHONY : clean_out  
clean_out:
	    @echo 'removeing all *.o file ...'
	    @$(RM) -r $(pes_parent_dir)/build/*.o
	    @echo 'done'

#删除 build 文件夹
.PHONY : clean_build  
clean_build:
	    @echo 'removeing all build temp file ...'
	    @$(RM) -r $(pes_parent_dir)/build
	    @echo 'done'

#删除 可执行文件
.PHONY : clean_bin  
clean_bin:
	    @echo 'removeing executable file ...'
	    @$(RM) $(Bin)
	    @echo 'done'

		
#重新编译
.PHONY : rebuild
rebuild: clean_out AllLibs

#显示帮助
.PHONY : help
help:
		@echo "本makefile 一共实现了以下几种命令模式"
		@echo "AllLibs \t\t默认的命令,含义为编译当前项目并输出可执行文件 $(target)\n"
		@echo "clean \t\t清理命令,删除由makefile带来的所有文件\n"
		@echo "clean_out \t\t清理中间文件命令,删除由编译带来的所有文件 *.o\n"
		@echo "clean_build \t\t清理中间文件命令,删除编译临时文件夹 /build\n"
		@echo "clean_bin \t\t删除可执行文件 $(target)\n"
		@echo "rebuild \t\t重新编译\n"
		@echo "|----------------------------------------------|"
		@echo "内建一些逻辑,以及传参即可改变\n"
		@echo "ARGS \t\t用户自我追加的一些编译参数,例如多线程中需要加入 -lpthread 参数\n"
		@echo "TYPE \t\t用于确认当前项目是C++ 项目还是 C语言项目 (默认为C) 如果是C++ 那么自动编译*.cpp文件 ,如果是 C 那么自动编译*.c文件,暂不支持混合添加\n"
		@echo "target \t\t用户自定义生成的可执行文件名,例如windows下 生成obj.exe Linux下生成 obj.out\n"
		@echo "DEBUG \t\t一般传入 -g 表示支持单步调试,\n"
		@echo "PREDEF \t\t预先定义一些宏,在程序中起到开关裁剪的作用\n"
		@echo "Please check the console encoding format in case of garbled codes. The initial encoding format of this makefile is UTF8\n"


  ```


  最后makefile 项目已经上传到 GitHub [点击这里](https://github.com/KimAlittleStar/vscode_c_demo)