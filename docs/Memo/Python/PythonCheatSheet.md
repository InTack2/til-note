# Pythonチートシート
## scriptのフォルダを取得する
``` python
import os
script_path = os.path.dirname(__file__)
```


## 可変長引数でアトリビュートに格納する
``` python
class Sample(object):
    def __init__(self, **kwargs):
        for key, value in kwargs.items():
            setattr(self, key, value)
```

## cmds周りのnull回避
``` python
import maya.cmds
select_object = cmds.ls(select=True) or ""
```

https://speakerdeck.com/twada/write-code-every-day?slide=9