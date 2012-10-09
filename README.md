waf-learning
============

## waf 官网

http://code.google.com/p/waf/

## waf 构建 - Minimal
 
```shell
git clone https://code.google.com/p/waf/
cd waf
python waf-light
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