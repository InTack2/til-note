# テストでバージョンアップに備える

## 概要
毎回ツールのアップデート確認するのは大変なので、  
toxを使って一括でMayaのバージョンのpytestが行える様にしてみました。  

コツコツ今からでもテストを書いて、  
CIでtoxを回し優雅にMayaのアップデートに備えるべし！

### この内容で達成できること
- mayapyでのテスト実行
- 各Mayaバージョンでのテスト実行
- プログラムのロジック部分のテスト

### 達成できないこと
- GUI部分のテスト(mayapyの為、不可)

## サンプルリポジトリはこちら

<a href="https://github.com/InTack2/tox-sample"><img src="https://github-link-card.s3.ap-northeast-1.amazonaws.com/InTack2/tox-sample.png" width="460px"></a>

## そもそもテストとは
こちらが非常にわかりやすいです。  
テスト駆動開発でとても有名な方の解説。  
[50 分でわかるテスト駆動開発](https://channel9.msdn.com/Events/de-code/2017/DO03)


## TA的にテストは何が嬉しい？
色々なメリットはありますが、主に3点  
- コードのクオリティーが上がりやすい
  - テスト書きづらい→一つのクラスでいろいろやりすぎ、設計があんまり
- テストコード自体が**生きるドキュメント**になる
- Mayaのバージョンアップ時に**テストを回すだけであらかた正常に動いているツールがわかる**


## 開発環境
- Windows10 Home
- Maya2020, 2019, 2018(どれか一つ or 全部も可)

## 実行方法
実際にサンプルリポジトリを解説しつつ、進めていきます。

### サンプルリポジトリをクローンする
クローン後、クローンしたフォルダにカレントディレクトリを移動します。
``` bash
# Documents以下にクローンした場合
# C:\Users\InTack2\Documents>

cd tox-sample
# C:\Users\InTack2\Documents\tox-sample>
```

### Python2.7をインストールする
Mayaのmayapyにバージョンを合わせてインストールしてください。  
そこまで神経質にならなくてもマイナーバージョン位であればそこまで影響ないと思いますが、念のため。  
(Maya2020なら[Python 2.7.11](https://www.python.org/downloads/release/python-2711/))

インストールしたpythonに「tox」をインストール
``` bash
python -m pip install tox
```

### テストするMayaに合わせてtoxの設定を変更する
tox.iniを開きテストを行いたいバージョンのコメントアウトを外してください。  
※存在しないmayapyはコメントアウトしないとエラーになるので忘れずに

``` bash
envlist = 
    ; py38
    ; py27
    mayapy20
    ; mayapy19 ←;を外して有効にする
    ; mayapy18
```

### toxを実行する
``` bash
python -m tox
```

しばらくすると下記のような結果となれば成功です:)  

[![Image from Gyazo](https://i.gyazo.com/4ac90ad001c4dd3b54f53658cb12c882.png)](https://gyazo.com/4ac90ad001c4dd3b54f53658cb12c882)

内部的にはmayapyを使用し、テスト実行できています。

## 解説
執筆中