waf-learning
============

waf 官网
--------

http://code.google.com/p/waf/

waf 构建 - Minimal
------------------
 
```
$ git clone https://code.google.com/p/waf/
$ cd waf
$ python waf-light
```

即可产出一个独立的 waf 文件.

waf 构建脚本 wscript - Minimal
------------------------------

wscript 是一个 Python 脚本，描述构建过程。

```python
def options(opt):
    pass


def configure(cnf):
    pass


def build(bld):
    pass
```

每个函数就是一个步骤。configure 和 build 是最基本的不可缺少的步骤。options 步骤是提供编译参数控制的，可选但通常都会需要。

waf 后面加步骤名就可以执行相应的步骤。因为 build 步骤太常用了，如果不写步骤名直接调用 waf ，默认就是执行 build 步骤。

调用顺序是： configure -> build 。

```
$ waf configure
Setting top to                           : /Users/leemars/Workspace/waf-learning/ut 
Setting out to                           : /Users/leemars/Workspace/waf-learning/ut/build 
'configure' finished successfully (0.119s)
$ waf
Waf: Entering directory `/Users/leemars/Workspace/waf-learning/ut/build'
Waf: Leaving directory `/Users/leemars/Workspace/waf-learning/ut/build'
'build' finished successfully (0.004s)
```

wscript 基础
============

单文件工程: hello world
-----------------------

### 任务

只有一个 C++ 程序 hello.cpp 文件 ，要求把这个文件编译成可执行文件。

### 构建脚本

```python
def options(opt):
    opt.load('compiler_cxx')


def configure(cnf):
    cnf.load('compiler_cxx')


def build(bld):
    bld.program(target='hello', source='hello.cpp')
```

在 options 和 build 阶段，通过 `load` 加载 `compiler_cxx` 这个 waf 的 Tools，用于编译 C++ 程序。如果要编译 C 程序，可以对应加载 `compiler_c`。

在 build 阶段，通过 `program` 和指定 `target` 以及 `source`，就从 hello.cpp 生成了 hello。所有产出均放在 build/ 目录下。

### 构建

```
$ waf --help
waf [commands] [options]

... ignore ...

  C++ Compiler Options:
    --check-cxx-compiler=CHECK_CXX_COMPILER
                        On this platform (darwin) the following C++ Compiler
                        will be checked by default: "g++"
$ waf configure
Setting top to                           : /Users/leemars/Workspace/waf-learning/ex1 
Setting out to                           : /Users/leemars/Workspace/waf-learning/ex1/build 
Checking for 'g++' (c++ compiler)        : /usr/bin/g++ 
'configure' finished successfully (0.044s)
$ waf build
Waf: Entering directory `/Users/leemars/Workspace/waf-learning/ex1/build'
[1/2] cxx: hello.cpp -> build/hello.cpp.1.o
[2/2] cxxprogram: build/hello.cpp.1.o -> build/hello
Waf: Leaving directory `/Users/leemars/Workspace/waf-learning/ex1/build'
'build' finished successfully (0.115s)
$ ./build/hello
Hello, world
```

多文件工程：calculator
----------------------

### 任务

C 工程，将 add.c 编译成动态链接库 add ，将 sub.c 编译成静态链接库 sub ，编译 calc.c 并链接 add 与 sub 生成 calc 。

### 构建脚本

```python
def options(opt):
    opt.load('compiler_c')


def configure(cnf):
    cnf.load('compiler_c')


def build(bld):
    bld.shlib(target='add', source='add.c')
    bld.stlib(target='sub', source='sub.c')
    bld.program(target='calc', source='calc.c', use='add sub')
```

`shlib` 是 shard library 的缩写，用于生成动态链接库。`stlib` 是 static library 的缩写，用于生成静态链接库。在 `program` 时，用 `use` 指定需要链接的库。注意：通过 `use` 指定的是项目内部生成的库，如果需要链接系统提供的库，需要使用另外的关键字。

### 构建

```
$ waf configure
Setting top to                           : /Users/leemars/Workspace/waf-learning/ex2 
Setting out to                           : /Users/leemars/Workspace/waf-learning/ex2/build 
Checking for 'gcc' (c compiler)          : /usr/bin/gcc 
'configure' finished successfully (0.299s)
$ waf build
Waf: Entering directory `/Users/leemars/Workspace/waf-learning/ex2/build'
[1/6] c: add.c -> build/add.c.1.o
[2/6] c: sub.c -> build/sub.c.2.o
[3/6] c: calc.c -> build/calc.c.3.o
[4/6] cstlib: build/sub.c.2.o -> build/libsub.a
[5/6] cshlib: build/add.c.1.o -> build/libadd.dylib
[6/6] cprogram: build/calc.c.3.o -> build/calc
Waf: Leaving directory `/Users/leemars/Workspace/waf-learning/ex2/build'
'build' finished successfully (1.000s)
```