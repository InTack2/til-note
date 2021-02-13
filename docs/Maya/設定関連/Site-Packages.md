# Maya環境は汚さずにPythonライブラリを使用する。
## 概要
Mayaに直接パッケージをインストール等はせず、Maya起動時にバッチなどでPYTHONPATHを追加して使用する方法。  
結構一般的かもしれないが、忘れない様にまとめ。

### 前提環境
- Windows10
- Maya2020(mayapy2.7)

### 手順

#### Mayaにpipをインストールする
[AutodeskMAYAにpip, numpy をインストールする方法](https://qiita.com/kodai100/items/4ddd76145c11d8099c17)  
mayaはpython2.7(以降mayapy)なので、  
一見通常のpython2.7をインストールしてそこからインストールしても良さそうですが、  
該当するdllがなかったりしてnumpyなどはエラーとなります。  
そのため、mayapy自体にpipのインストールを推奨します。  

#### mayapyへの環境変数のpathを通す
今後利用する場合、あらかじめPATH通しておくと楽です。  
デフォルト  
"C:\Program Files\Autodesk\Maya2020\bin"  

``` bash
::通す前
"C:\Program Files\Autodesk\Maya2020\bin\mayapy.exe" -m pip freeze

::通した後
mayapy -m pip freeze
```

#### パッケージを直接インストールせず、--targetフラグで別のフォルダへインストールする
直接インストールすると配布環境全てでインストールする必要が出てくるため、  
--targetフラグでターゲットフォルダへインストールする。  
("C:\Users\Tack2\Documents\"の部分は各自環境に置き換え)  
``` bash
::例
mayapy -m pip install pytest -t "C:\Users\Tack2\Documents\maya2020-sitepacages"
```

#### バッチファイル等で対象のフォルダを追加した状態でMayaを起動する
``` bash
set PYTHONPATH="%PYTHONPATH%;C:\Users\Tack2\Documents\maya2020-sitepacages;"

maya
```
