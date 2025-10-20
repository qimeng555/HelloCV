在c/c++项目使用或编译时，不同的操作系统需要不同的构建文件，而cmake则是用一种统一的、跨平台的的配置语言（CMakeLists.txt)来描述项目构建过程，之后CMake会根据对应平台，生成对应的、原声惯用的构建文件。

《工作机制》

配置与生成的两步模型

![](https://cdn.nlark.com/yuque/0/2025/jpeg/61445279/1760845587503-d252ff06-5107-4a60-a4cd-53bd1a6385b3.jpeg)

ok这就是流程，文字介绍

运行cmake时，配置

//开始配置CMake读取源代码目录顶层的CMakeList.txt文件

//逐条执行命令（Project[定义项目名称】、add_executable[定义要构建的执行项目和源文件】、find_package【查找系统是否安装了所需的第三方库，如Opencv】

//检查环境（编译器、链接器、操作系统架构、依赖库）写入CMakeLise.txt文件，下次配置时直接调用

//生成内部模型：生成完整的、代表整个项目的结构的模型（库、行文件、依赖关系等）

生成，根据制定的生成器，来创建对应的构建文件。

//而在linux上，通常生成Unix Makefiles,得到Makefiles文件以后，只需要输入make，原生的make工具就会读取Makefiles来调用编译器，编译和链接你的代码

总之，CMake本身不编译代码，知识元构建系统，负责生成另一个构建系统所需的输入文件。CMakelList.txt类似于说明书，描述源文件在那里，依赖什么等，而CMake则是读取前者，检索环境并生成适合当前的构建文件，最后原生构建工具就是真正开始编译，生成可执行的文件或库



《编写命令》

//cmake_minimum_required(VERSION<min>[<max>])

设置处理当前CMakelist.txt所需CMake的最低（和可选的最大）版本

！必须是文件第一行



//project(<PROJECT-NAME>[LANGUAGES] [VERSION] [DESCRIPTION])

定义项目的名称和其他元信息

例子：project(MyAwesomeProject LANGUAGES C CXX VERSION 1.0.0 DESCRIPTION "A great project")

定义了一个项目名，支持C和C++，设置版本和描述



//add_executable(<name>[source1] [source2]...)

告诉CMake要从指定的源文件列表构建一个可执行文件

参数：<name>想要可执行文件的目标名称

[sourcenN]:构建此可执行文件所需的所有源文件



//add_library(<name>[STATIC | SHARED | MODULE] [sourceN])

让CMake从源文件构建一个库



//target_link_libraries(<target><PRIVATE |PUBLIC |INTERACE><item>....)

指定一个目标需要链接哪些库标

关键字（PRIVATE是仅用于构建当前目标、|PUBLIC也可以用于使用当前目标的目标  INTERACE仅用于使用当前目标的目标，而当前目标不使用它）



//include_directories([AFTER | BEFORE][SYSTEM][dirN])

将指定目录添加到所有目标的头文件搜索路径中



//add_subdirectory(source_dir[binary_dir])

用于告诉CMake进入指定的子目录，找到并执行该目录下的CMakeList.txt文件（这可以将项目的不同部分放在不同的文件夹，每个文件夹管理自己的构建规则）

MyProject/

├── CMakeLists.txt          # 根CMakeLists

├── src/

│   ├── CMakeLists.txt      # 主程序模块

│   └── main.cpp

├── lib/

│   ├── CMakeLists.txt      # 库模块

│   ├── math_utils.cpp

│   └── math_utils.h

└── tests/

    ├── CMakeLists.txt      # 测试模块

    └── test_math.cpp

这个便是我用deepseek弄出的较为形象的项目结构



//target_include_directories(<target><scope><dir>)

指定目标的头文件搜索路径，PUBLIC:目标自身及依赖它的其他目标都能使用该路径（常用库暴露头文件）PRIVATE：仅目标自身可用（用于可执行文件隐藏内部头文件）



//find_package(OpenCV REQUIRED):自动查找系统中的OpenCV，（REQUIRES表示中找不到直接报错），找到后，CMake会自动定义OpenCV_INCLUDE_DIRS(头文件路径）和OpenCV_LIBS（库文件路径）两个变量



```plain
cmake_minimum_required(VERSION 3.10)
project(MyProject VERSION 1.0 LANGUAGES CXX)

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

# 第一步：先查找第三方库OpenCV（必须在子模块前，确保子模块能复用变量）
find_package(OpenCV REQUIRED)
# 可选：打印找到的OpenCV信息，用于调试
message(STATUS "OpenCV版本: ${OpenCV_VERSION}")
message(STATUS "OpenCV头文件: ${OpenCV_INCLUDE_DIRS}")
message(STATUS "OpenCV库: ${OpenCV_LIBS}")

# 第二步：关联子模块（顺序：库→主程序→测试）
add_subdirectory(lib)
add_subdirectory(src)
```

ok这是我借助ai工具弄得一个多模块+OpenCV链接的例子，以上的是根目录CMakeLise.txt的编写，这样会使得后续自定义库在链接OpenCV职后，主程序只须链接自定义库即可，无需关注库的依赖细节。



《源外码的构建》

他时将编译生成的中间文件（如Makefile、目标文件、科执行程序）全部放入独立的build目录中

第一步：mkdir build 创建独立编译目录

第二步：cd build 进入编译目录

第三步：cmake ..  生成编译规则

第四步：make 执行编译 （读取build目录下的makefile，编译生成文件等）

优点：使得原始代码不被污染，目录更清晰；多版本编译互不干扰；可一键删除目录（make clean)



《安装和配置》

首先install（）是CMake中用于定义安装规则的核心指令，能将编译后的产物按自定义路径安装到系统或指定目录

```plain
add_library(math_utils STATIC math_utils.cpp math_utils.h)
target_include_directories(math_utils PUBLIC ${CMAKE_CURRENT_SOURCE_DIR})

# 安装库文件：ARCHIVE 对应静态库（.a/.lib），LIBRARY 对应动态库（.so/.dll）
install(TARGETS math_utils ARCHIVE DESTINATION lib  # 静态库→安装到 ${CMAKE_INSTALL_PREFIX}/lib
)
add_executable(MyApp main.cpp)
target_link_libraries(MyApp PRIVATE math_utils)

# 安装可执行文件：TARGETS 指定目标，DESTINATION 指定安装目录
install(TARGETS MyApp RUNTIME DESTINATION bin  # 可执行文件→安装到 ${CMAKE_INSTALL_PREFIX}/bin
)
# 安装整个目录：DIRECTORY 指定源目录，DESTINATION 指定目标目录
install(DIRECTORY config/
    DESTINATION share/MyProject/config  # 配置文件→安装到 ${前缀}/share/MyProject/config
)
# 安装头文件：FILES 指定文件，DESTINATION 指定安装目录
install(FILES math_utils.h
    DESTINATION include  # 头文件→安装到 ${CMAKE_INSTALL_PREFIX}/include
)
```

由于实在太多，在理解后我便将代码直接copy到此处了，以后看见相同的也能够即使回忆到。



《构建问题排查》

1.路径找不到

fatal error:math_utils.h:No such file or directory(头文件找不到）

Cannot find source file xxx.cpp(源文件路径错）

Could not find CMakeList.txt in directory:xxx(add_subdirectory路径错)

对于头文件而言，检查target_include_directories的路径是否正确

若引用子模块头文件，确保路径是PUBLIC${CMAKE_CURRENT_SOURCE_DIR}(当前目录）或../lib(相对途径）

对于源文件/子目录途径的问题，检查add_executable/add_library中源文件名称是否正确（大小写），检查add_subdirectory的目录是否存在，路径是否正确

2.依赖找不到（第三方库/自定义库），对于第三方库，首先确认库已经安装（即验证依赖存在于目录）指定库路径（cmake .. -DOpenCV_DIR=~/opencv/share/OpenCV  # 指向含 OpenCVConfig.cmake 的目录），最后是检查命令的语法；对于自定义库，首先确认库已经编译，接下来检查链接顺序（按“依赖顺序”）最后是检查target_link_libraries的正确使用

3.版本不兼容

CMake 3.15 or higher is requires.You are running version 3.10(版本太低）

error: "std::fileystem"has not been declared(c++标准不匹配）

compliation terminated due to -Wfatal -errors(编译器版本不兼容）

升级（降低）CMake；检查CMakeList.txt中是否指定正确的c++标准；升级编译器sudo apt install g++-9n，并通过export CXX=g++-9制定 



其实大多时候我认为可以用ai来辅助进行排查，但是必要的排查技巧是要有的，如：cmake使加-DCMAKE_VERSION_MAKEFILE=ON,编译时make VERBOSE=1，查看完整编译指令，定位错误路径。

此外，还有一个我从之前配置vscode的c++时报错得到的方法：清理重建,rm -rf build 删除目录之后再重新构建。



总之呢，作为一个强大的工具，cmake在报错时有时会有点麻烦，这时不妨一步步运行，缩小范围来确定位置。





















