# PyPIにアップロードするまでに必要なこと
PythonのモジュールをPyPIにあげるまでに必要なことをまとめます。

## setup.py setup.cfgの作成
まずパッケージ化に必要な情報をまとめるsetup.pyやcfgを作成します。  
個人的にはsetup.pyだとごちゃっとなるので、  yaml,ini風のcfgの方に移動するのが好みです。  

## setup.pyからビルドする
``` bash
python setup.py sdist bdist_wheel
```

## setup.py develop
https://qiita.com/edvakf@github/items/d82cd7ab77ea2b88506c


## CLIコマンドパッケージ
entry_points
https://www.karakaram.com/how-to-create-python-cli-package/
