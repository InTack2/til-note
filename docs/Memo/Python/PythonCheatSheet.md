# Pythonチートシート

## パス
### scriptのフォルダを取得する
``` python
import os
script_path = os.path.dirname(__file__)
```

## コンストラクタ
### 可変長引数でアトリビュートに格納する
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

## インポート関連

### Importされている物を確認する
``` python
import sys

print(sys.modules)
```

### インポートされている物を正規表現で確認で検索する
``` python
import sys
import re
import pprint

REGEX = re.compile(r"検索対象")


hit_module_names = []
for module_name in sys.modules:
    if REGEX.search(module_name):
        hit_module_names.append(module_name)

pprint.pprint(hit_module_names)
```