# PythonでCLI上で対話的にユーザー入力させる

こんな感じのやつが可能。
[![Image from Gyazo](https://i.gyazo.com/234794e430c4ab5945eb005b98946550.gif)](https://gyazo.com/234794e430c4ab5945eb005b98946550)

## Python Package
有名どころで下記の2点  

- [Inquirer](https://pypi.org/project/inquirer/)
- [PyInquirer](https://pypi.org/project/PyInquirer/)

### 現状
InquirerはWindowsではいくつかつらい不具合あり  
(cmd以外でリストが移動できない、全般的に入力後の文字が消せないなど..)  
PyInquirerはその様な不具合がなく  
Windows Terminal、cmder、cmdなどでも正常に動作する為、  
個人敵にPyInquirerをおすすめ  


## PyInquirer基本的な使い方
基本的にはInquirer、PyInquirerのどちらも  
node.jsのInquirerを参考にしている為、似たような書き方で実現可能。

なぜかPyInquirer公式のexamplesがエラー出る為、実際の動きとサンプルを記載。


### Yes/No形式
～ですか？などbool値で分岐等に  
[![Image from Gyazo](https://i.gyazo.com/b10aec66cc86083e0f107ea639fb9ab7.gif)](https://gyazo.com/b10aec66cc86083e0f107ea639fb9ab7)
``` python
# -*- coding: utf-8 -*-
from __future__ import print_function
from __future__ import unicode_literals
from __future__ import absolute_import
from __future__ import generators
from __future__ import division
from PyInquirer import prompt

question_description = [
    {
        "type": "confirm",
        "message": "CLIの対話PyInquirerの紹介を見ますか？",
        "name": "is_description",
        "defalut": True
    },
    {
        "type": "confirm",
        "message": "本当にみますか？",
        "name": "is_description",
        "defalut": False,
        "when": lambda answers: answers["is_description"]
    }
]
answers = prompt(question_description)
print(answers)
```

### テキスト入力形式
デフォルト値なども追加可能。  
内容が日本語などだとCUIによってはUnicodeエスケープ表示になるやも  

[![Image from Gyazo](https://i.gyazo.com/ad027b1113ab84a92cffcab7c92efa49.gif)](https://gyazo.com/ad027b1113ab84a92cffcab7c92efa49)
``` python
# -*- coding: utf-8 -*-
from __future__ import print_function
from __future__ import unicode_literals
from __future__ import absolute_import
from __future__ import generators
from __future__ import division
from PyInquirer import prompt

questions_text = [
    {
        "name": "question_1",
        "type": "input",
        "message": "質問１",
        "default": "デフォルトの値",
    },
    {
        "name": "question_2",
        "type": "input",
        "message": "質問2",
        "default": "デフォルトの値",
    },
]
answers = prompt(questions_text)
print(answers)
```

### リスト
一定の選択項目から選択  
ただのリストで渡しているだけなので、自動生成した選択項目など入れやすい  
[![Image from Gyazo](https://i.gyazo.com/64a5b375b204f47fd1a5e28146fc143b.gif)](https://gyazo.com/64a5b375b204f47fd1a5e28146fc143b)
``` python
# -*- coding: utf-8 -*-
from __future__ import print_function
from __future__ import unicode_literals
from __future__ import absolute_import
from __future__ import generators
from __future__ import division
from PyInquirer import prompt

question_list = ["sample_01", "sample_02", "sample_03"]
questions_list = [
    {
        "type": "list",
        "message": "リスト表示",
        "name": "language",
        "choices": question_list
    },
]
answers = prompt(questions_list)
print(answers)

```

### チェックリスト
++space++でチェック、  
(ショートカット++a++でトグル、++i++で反転)
[![Image from Gyazo](https://i.gyazo.com/b9a09d4f02a5f6852e0b6de33d72ee76.gif)](https://gyazo.com/b9a09d4f02a5f6852e0b6de33d72ee76)
``` python
# -*- coding: utf-8 -*-
from __future__ import print_function
from __future__ import unicode_literals
from __future__ import absolute_import
from __future__ import generators
from __future__ import division

from pprint import pprint

from PyInquirer import prompt, Separator

questions = [
    {
        "type": "checkbox",
        "message": "チェックボックスサンプル",
        "name": "check_box_sample",
        "choices": [
            Separator("= separator="),
            {
                "name": "sample_01"
            },
            Separator("= 特殊な操作 ="),
            {
                "name": "sample_02"
            },
            {
                "name": "sample_03"
            },
        ]
    }
]
answers = prompt(questions)
print(answers)

```

### Tips
#### whenによる組み合わせ
一つ前の以前の質問がTrueなら質問を続行したり質問、リスト等と混ぜることができる。  
答えが辞書で返ってくるので、自前で分岐等も可能ですが、  
一式質問等でまとめたりなどが可能。

[![Image from Gyazo](https://i.gyazo.com/69e1be7297956ee7b49a71c220afbe79.gif)](https://gyazo.com/69e1be7297956ee7b49a71c220afbe79)
``` python
from PyInquirer import prompt

question_list = ["sample_01", "sample_02", "sample_03"]
question_description = [
    {
        "type": "confirm",
        "message": "CLIの対話PyInquirerの紹介を見ますか？",
        "name": "is_description",
        "defalut": True
    },
    {
        "type": "list",
        "message": "リスト表示",
        "name": "language",
        "choices": question_list,
        "when": lambda answers: answers["is_description"]
    },
]
answers = prompt(question_description)
print(answers)
```