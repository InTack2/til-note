# PyPIにアップロードするまでに必要なこと
PythonのモジュールをPyPIにあげるまでに必要なことをまとめます。

## setup.py setup.cfgの作成
まずパッケージ化に必要な情報をまとめるsetup.pyやcfgを作成します。  
個人的にはsetup.pyだとごちゃっとなるので、  yaml,ini風のcfgの方に移動するのが好みです。  

### setup.pyの書き方
執筆中  

### setup.cfgの書き方
執筆中  

## アップロードに必要なパッケージをインストールする
``` bash
python -m pip install wheel twine
```

## setup.pyからビルドする
``` bash
python setup.py sdist bdist_wheel
```

## testPyPIにアップする
まずは直接PyPIにアップするのではなく、Test用のPyPIを推奨。  
``` bash
twine upload --repository testpypi dist/*
```

## PyPIにアップする
``` bash
twine upload --repository pypi dist/*
```


## 参考
- CLIコマンドパッケージを作る時の参考  
entry_pointsなどなど  
[https://www.karakaram.com/how-to-create-python-cli-package/](https://www.karakaram.com/how-to-create-python-cli-package/)
