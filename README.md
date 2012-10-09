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
def configure(ctx):
    pass


def build(ctx):
    pass
```

每个函数就是一个步骤。configure 和 build 可以认为是最基本的不可缺少的步骤。

waf 后面加步骤名就可以执行相应的步骤。因为 build 步骤太常用了，如果不写步骤名直接调用 waf ，默认就是执行 build 步骤。

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