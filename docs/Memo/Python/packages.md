# PyPIセットアップ
PythonのモジュールをPyPIにあげるまでに必要なことをまとめます。

## setup.py setup.cfgの作成
まずパッケージ化に必要な情報をまとめるsetup.pyやcfgを作成します。  
個人的にはsetup.pyだとごちゃっとなるので、  yaml,ini風のcfgの方に移動するのが好みです。  

### setup.pyの書き方
実際に個人的に作成しているツールのsetup.pyを参考に。  
基本的に何かPythonでやる必要があるアトリビュート以外は全てcfg側に記載しています。  
```python
from setuptools import setup
import re
import io

__version__ = re.search(
    r'__version__\s*=\s*[\'"]([^\'"]*)[\'"]',  # It excludes inline comment too
    io.open('src/boip/__init__.py', encoding='utf_8_sig').read()).group(1)

setup(
    name="Boip",
    version=__version__,
)
```

### setup.cfgの書き方
長くなりましたが、実際のcodeを記載。  
下にひとつずつ説明を書いていきます。  
```ini
[metadata]
name = boip
description = A tool for creating Boiler Plates.
url = https://github.com/InTack2/boip
author = Tack2
author_email = takumi236@gmail.com
license = MIT
license_file = LICENSE
long_description = file: README.md
long_description_content_type = text/markdown
classifiers =
    Development Status :: 1 - Planning
    Environment :: Console
    Intended Audience :: Developers
    Intended Audience :: Developers
    License :: OSI Approved :: MIT License
    Programming Language :: Python :: 2
    Programming Language :: Python :: 2.7
    Programming Language :: Python :: 3
    Programming Language :: Python :: 3.5
    Programming Language :: Python :: 3.6
    Programming Language :: Python :: 3.7
    Programming Language :: Python :: 3.8
    Operating System :: Microsoft :: Windows :: Windows 10
    Natural Language :: Japanese
keywords = PyInquirer, Inquirer, Maya
project_urls =
    Source=https://github.com/InTack2/boip
    Tracker=https://github.com/InTack2/boip/issues

[options]
zip_safe = False
packages = find:
package_dir = = src
include_package_data = true
python_requires = >=2.7, !=3.0.*, !=3.1.*, !=3.2.*, !=3.3.*, !=3.4.*
install_requires =
    pyyaml>=5.3.1
    PyInquirer>=1.0.3
tests_require =
    pytest
    tox

[options.packages.find]
where = src

[aliases]
test=pytest

[options.entry_points]
console_scripts =
    boip = boip.cli:_main

[tool:pytest]
testpaths = ./tests
python_files = test_*.py
python_classes = Test
python_functions = test_
```
### [metadata]
PyPIにアップするときにPyPIに認識させる情報。
直観的にわかりづらい、これ覚えるの大変という所を記載します。  

#### classifiers
PyPIで検索する時のタグなどを指定します。  
大量にあるので、[こちら](https://pypi.org/classifiers/)のサイトがおすすめ。  

### [options]
インストールする時の設定などを記載。  

### [options.packages.find]
下記のように記載するとsrc以下にある物をパッケージとして認識します。  
```ini
[options.packages.find]
where = src
```

### [options.entry_points]
エントリーポイントです。  
pytestなどインストールすると  
「python -m pytest」と入力せずとも「pytest」で実行可能になりますが、これがこちらの設定となります。  
下記の例は「boip」と入力するとboip.cliというファイルの_mainを実行するという意味になります。  

```ini
[options.entry_points]
console_scripts =
    boip = boip.cli:_main
```

## アップロードに必要なパッケージをインストールする
wheelはpython2,3系をまとめたパッケージを生成する  
比較的新しい形式のパッケージファイルを生成する為のライブラリ。  
  
twineはPyPIにアップする為のライブラリです。  

``` bash
python -m pip install wheel twine
```

## setup.pyからビルドする
``` bash
python setup.py sdist bdist_wheel
```

## testPyPI、PyPIに登録する
両方共ユーザー登録が必要です。  
Pythonコマンドでアップする時にユーザー名とパスワード聞かれます。  
- [testPyPI](https://test.pypi.org/account/register/)  
- [PyPI](https://pypi.org/account/register/)  


## PyPIにアップする
!!! Note
    まずは直接PyPIにアップするのではなく、Test用のPyPIを推奨します。  
!!! Warning
    PyPI,testPyPI共に、一度アップしたバージョンを再度アップする事はできません。  
    アップ後はversionを変更してアップしてください。  
    PyPIとtestPyPIは完全に別なので、双方で違っていても問題ありません。  
### testPyPI
``` bash
twine upload --repository testpypi dist/*
```

### PyPI
``` bash
twine upload dist/*
```


## 参考
- CLIコマンドパッケージを作る時の参考  
entry_pointsなどなど  
[https://www.karakaram.com/how-to-create-python-cli-package/](https://www.karakaram.com/how-to-create-python-cli-package/)
