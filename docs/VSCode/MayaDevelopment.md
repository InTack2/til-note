# Maya用開発情報
## 概要
VSCodeのMaya開発時に必要な情報まとめ

## 外部フォルダへのPathを通す
### 背景
Mayaの場合、パッケージを直接インストールすると配布後に各環境にインストールする必要がある為、  
外部フォルダにインストールしていたりする。  
[こんなイメージ](../../Maya/sitepackages.md)  

### 問題点
もちろんそんな事VSCodeは知らないので、
mayapyをインタープリターに設定してもそんなパッケージないよエラーが表示される  
それにFlake8やPytestをOnにしているとパッケージがないよ、インストールする？と聞かれてしまう  
(それに気軽にOKしてしまうとmayapyにpip install を開始する。やさしさだね。)

### 回避方法
いくつ方法があるが、この方法が一番簡単で問題がなくおすすめ  

#### .envファイルを作成する
下記の様にプロジェクト直下に.envファイルを作成する  
[![Image from Gyazo](https://i.gyazo.com/83966090124c98b3a0caa7191b09b2da.png)](https://gyazo.com/83966090124c98b3a0caa7191b09b2da)

#### .envファイル内にフォルダへのパスを追加する  
``` bash
PYTHONPATH="${env:PYTHONPATH};追加したいsite-packagesへのパス"
```
#### vscodeを再起動する
.envの更新すると自動的に更新されるわけではないので、再起動が必要。  

#### vscodeからの警告やエラーを確認する  
Flake8のエラーやimport文記載時のエラーがなくなっていれば完了です。  

