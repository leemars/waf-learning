waf-learning
============


## waf 官网

http://code.google.com/p/waf/


## waf 构建 - Minimal
 
```
$ git clone https://code.google.com/p/waf/
$ cd waf
$ python waf-light
```

即可产出一个独立的 waf 文件.


## waf 构建脚本 wscript - Minimal

wscript 是一个 Python 脚本.

```python
# wscript

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


## wscript 基础


### 单文件工程: hello world

任务：只有一个 C++ 程序 hello.cpp 文件 ，要求把这个文件编译成可执行文件。

hello.cpp 文件：
```cpp
#include <stdio.h>

int main() {
    puts("Hello, world");
    return 0;
}
```

wscript 文件：
```python
def options(opt):
    opt.load('compiler_cxx')


def configure(cnf):
    cnf.load('compiler_cxx')


def build(bld):
    bld.program(source='hello.cpp', target='hello')
```

构建：
```
$ waf --help
waf [commands] [options]

... ignore ...

  C++ Compiler Options:
    --check-cxx-compiler=CHECK_CXX_COMPILER
                        On this platform (darwin) the following C++ Compiler
                        will be checked by default: "g++"
$ waf configure
Setting top to                           : /Users/leemars/Workspace/waf-learning/ut 
Setting out to                           : /Users/leemars/Workspace/waf-learning/ut/build 
Checking for 'g++' (c++ compiler)        : /usr/bin/g++ 
'configure' finished successfully (0.424s)
$ waf build
Waf: Entering directory `/Users/leemars/Workspace/waf-learning/ut/build'
[1/2] cxx: hello.cpp -> build/hello.cpp.1.o
[2/2] cxxprogram: build/hello.cpp.1.o -> build/hello
Waf: Leaving directory `/Users/leemars/Workspace/waf-learning/ut/build'
'build' finished successfully (0.754s)
```