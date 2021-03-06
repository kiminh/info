Python在Windows上的常见问题
***************************


我怎样在Windows下运行一个Python程序？
=====================================

这不一定是一个简单的问题。如果你已经熟悉在Windows的命令行中运行程序的
方法，一切都显而易见；不然的话，你也许需要额外获得些许指导。

除非你在使用某种集成开发环境，否则你将会在被称为“DOS 窗口”或“命令提示
符窗口”的地方 *输入* Windows 命令。 通常你可以在搜索栏搜索 "cmd" 来创
建这种窗口。 你应该能够识别你何时打开了这样的窗口，因为你将看到一个
Windows“命令提示符”，通常看起来是这样：

   C:\>

前面的字母可能会不同，而且后面有可能会有其他东西，所以你也许会看到类似
这样的东西：

   D:\YourName\Projects\Python>

出现的内容具体取决与你的电脑如何设置和你最近用它做的事。 当你启动了这
样一个窗口后，就可以开始运行Python程序了。

Python 脚本需要被另外一个叫做 Python *解释器* 的程序来处理。 解释器读
取脚本，把它编译成字节码，然后执行字节码来运行你的程序。 所以怎样安排
解释器来处理你的 Python 脚本呢？

首先，确保命令窗口能够将“py”识别为指令来开启解释器。 如果你打开过一个
命令窗口， 尝试输入命令 "py" 然后按回车：

   C:\Users\YourName> py

然后你应当看见类似类似这样的东西：

   Python 3.6.4 (v3.6.4:d48eceb, Dec 19 2017, 06:04:45) [MSC v.1900 32 bit (Intel)] on win32
   Type "help", "copyright", "credits" or "license" for more information.
   >>>

解释器已经以“交互模式”打开。这意味着你可以交互输入Python语句或表达式，
并在等待时执行或评估它们。这是Python最强大的功能之一。输入几个表达式并
看看结果：

   >>> print("Hello")
   Hello
   >>> "Hello" * 3
   'HelloHelloHello'

许多人把交互模式当作方便和高度可编程的计算器。 想结束交互式Python会话
时，调用 "exit()" 函数，或者按住 "Ctrl" 键时输入 "Z"  ，之后按 "Enter"
键返回Windows命令提示符。

你可能发现在开始菜单有这样一个条目 开始 ‣ 所有程序 ‣ Python 3.x ‣
Python (命令行)， 运行它后会出现一个有着 ">>>" 提示的新窗口。 在此之后
，如果调用 "exit()" 函数或按 "Ctrl-Z" 组合键后窗口将会消失。 Windows
会在这个窗口中运行一个“python”命令，并且在你终止解释器的时候关闭它。

现在我们知道 "py" 命令已经被识别，可以输入Python脚本了。 你需要提供
Python脚本的绝对路径或相对路径。 假设Python脚本位于桌面上并命名为
"hello.py"，并且命令提示符在用户主目录打开，那么可以看到类似于这样的东
西:

   C:\Users\YourName>

那么现在可以让``py``命令执行你的脚本，只需要输入``py`` 和脚本路径:

   C:\Users\YourName> py Desktop\hello.py
   hello


我怎么让 Python 脚本可执行？
============================

在 Windows 上，标准 Python 安装程序已将 .py 扩展名与文件类型
(Python.File) 相关联，并为该文件类型提供运行解释器的打开命令
("D:\Program Files\Python\python.exe "%1" %*") 。 这足以使脚本在命令提
示符下作为“foo.py”命令被执行。 如果希望通过简单地键入“foo”而无需输入文
件扩展名来执行脚本，则需要将 .py 添加到 PATHEXT 环境变量中。


为什么有时候 Python 程序会启动缓慢？
====================================

通常，Python 在 Windows 上启动得很快，但偶尔会有错误报告说 Python 突然
需要很长时间才能启动。更令人费解的是，在其他配置相同的 Windows 系统上
，Python 却可以工作得很好。

该问题可能是由于计算机上的杀毒软件配置错误造成的。当将病毒扫描配置为监
视文件系统中所有读取行为时，一些杀毒扫描程序会导致两个数量级的启动开销
。请检查你系统安装的杀毒扫描程序的配置，确保两台机它们是同样的配置。已
知的， McAfee 杀毒软件在将它设置为扫描所有文件系统访问时，会产生这个问
题。


我怎样使用Python脚本制作可执行文件？
====================================

请参阅 cx_Freeze 了解 distutils 扩展，它允许你从 Python 代码创建控制台
和 GUI 可执行文件。 py2exe ，是构建基于 Python 2.x 的可执行文件的最流
行扩展，它还不支持 Python 3 ，但这个版本正在开发。


"*.pyd" 文件和DLL文件相同吗？
=============================

是的， .pyd 文件也是 dll ，但有一些差异。如果你有一个名为 "foo.pyd" 的
DLL，那么它必须有一个函数 "PyInit_foo()" 。 然后你可以编写 Python 代码
“import foo” ，Python 将搜索 foo.pyd （以及 foo.py 、 foo.pyc ）。如果
找到它，将尝试调用 "PyInit_foo()" 来初始化它。你不应将 .exe 与 foo.lib
链接，因为这会导致 Windows 要求存在 DLL 。

请注意， foo.pyd 的搜索路径是 PYTHONPATH ，与 Windows 用于搜索 foo.dll
的路径不同。此外， foo.pyd 不需要存在来运行你的程序，而如果你将程序与
dll 链接，则需要 dll 。 当然，如果你想 "import foo" ，则需要 foo.pyd
。在 DLL 中，链接在源代码中用 "__declspec(dllexport)" 声明。 在 .pyd
中，链接在可用函数列表中定义。


我怎样将Python嵌入一个Windows程序？
===================================

在 Windows 应用程序中嵌入 Python 解释器可以总结如下：

1. 请 _不要_ 直接在你的 .exe 文件中内置 Python 。在 Windows 上，
   Python 必须是一个 DLL ，这样才可以处理导入的本身就是 DLL 的模块。（
   这是第一个未记录的关键事实。）相反，链接到 "python*NN*.dll" ；它通
   常安装在 "C:\Windows\System" 中。 *NN* 是 Python 版本，如数字“33”代
   表 Python 3.3 。

   你可以通过两种不同的方式链接到 Python 。加载时链接意味着链接到
   "python*NN*.lib" ，而运行时链接意味着链接 "python*NN*.dll" 。（一般
   说明： "python *NN*.lib" 是所谓的“import lib”，对应于
   "python*NN*.dll" 。它只定义了链接器的符号。）

   运行时链接极大地简化了链接选项，一切都在运行时发生。你的代码必须使
   用 Windows 的 "LoadLibraryEx()" 程序加载 "python*NN*.dll" 。代码还
   必须使用使用 Windows 的 "GetProcAddress()" 例程获得的指针访问
   "python*NN*.dll" 中程序和数据（即 Python 的 C API ）。宏可以使这些
   指针对任何调用 Python C API 中的例程的 C 代码都是透明的。

   Borland 提示：首先使用 Coff2Omf.exe 将 "python*NN*.lib" 转换为 OMF
   格式。

2. 如果你使用 SWIG ，很容易创建一个 Python “扩展模块”，它将使应用程序
   的数据和方法可供 Python 使用。SWIG将为你处理所有蹩脚的细节。结果是
   你将链接到 .exe 文件 *中* 的C代码 (!) 你不必创建 DLL 文件，这也简化
   了链接。

3. SWIG 将创建一个 init 函数（一个 C 函数），其名称取决于扩展模块的名
   称。例如，如果模块的名称是 leo ，则 init 函数将被称为 initleo() 。
   如果您使用 SWIG 阴影类，则 init 函数将被称为 initleoc() 。这初始化
   了一个由阴影类使用的隐藏辅助类。

   你可以将步骤 2 中的 C 代码链接到 .exe 文件的原因是调用初始化函数等
   同于将模块导入 Python ！ （这是第二个关键的未记载事实。）

4. 简而言之，你可以用以下代码使用扩展模块初始化 Python 解释器。

      #include "python.h"
      ...
      Py_Initialize();  // Initialize Python.
      initmyAppc();  // Initialize (import) the helper class.
      PyRun_SimpleString("import myApp");  // Import the shadow class.

5. Python C API 存在两个问题，如果你使用除 MSVC 之外的编译器用于构建
   python.dll ，这将会变得明显。

   问题1：采用 FILE* 参数的所谓“极高级”函数在多编译器环境中不起作用，
   因为每个编译器的FILE结构体概念都不同。从实现的角度来看，这些是非常
   _低_ 级的功能。

   问题2：在为void函数生成包装器时，SWIG会生成以下代码：

      Py_INCREF(Py_None);
      _resultobj = Py_None;
      return _resultobj;

   Py_None 是一个宏，它扩展为对 pythonNN.dll 中名为 _Py_NoneStruct 的
   复杂数据结构的引用。同样，此代码将在多编译器环境中失败。将此类代码
   替换为：

      return Py_BuildValue("");

   有可能使用 SWIG 的 "%typemap" 命令自动进行更改，但我无法使其工作（
   我是一个完全的SWIG新手）。

6. 使用 Python shell 脚本从 Windows 应用程序内部建立 Python 解释器窗口
   并不是一个好主意；生成的窗口将独立于应用程序的窗口系统。相反，你（
   或 wxPythonWindow 类）应该创建一个“本机”解释器窗口。将该窗口连接到
   Python解释器很容易。你可以将 Python的 i/o 重定向到支持读写的 _任意_
   对象，因此你只需要一个包含 read() 和 write() 方法的 Python 对象（在
   扩展模块中定义）。


如何让编辑器不要在我的 Python 源代码中插入 tab ？
=================================================

本 FAQ 不建议使用制表符， Python 样式指南 **PEP 8** ，为发行的 Python
代码推荐 4 个空格；这也是 Emacs python-mode 默认值。

在任何编辑器下，混合制表符和空格都是一个坏主意。 MSVC 在这方面没有什么
不同，并且很容易配置为使用空格： 点击 Tools ‣ Options ‣ Tabs，对于文件
类型“Default”，设置“Tab size”和“Indent size”为 4 ，并选择“插入空格”单
选按钮。

如果混合制表符和空格导致前导空格出现问题， Python 会引发
"IndentationError" 或 "TabError" 。你还可以运行 "tabnanny" 模块以批处
理模式检查目录树。


如何在不阻塞的情况下检查按键？
==============================

使用 msvcrt 模块。 是标准的 Windows 特定扩展模块。它定义了一个函数
"kbhit()" 来检查是否存在键盘命中，而 "getch()" 来获取一个字符而不回显
它。
