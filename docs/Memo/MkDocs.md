# MkDocs
このサイト自体に使用している静的サイトジェネレーター


## 参考

[MkDocsによるドキュメント作成](https://qiita.com/mebiusbox2/items/a61d42878266af969e3c)

## 環境構築

``` bash
pipenv install
```

## 基本コマンド
個人的にpipenv使用なので、pipenv ~を記載してますが、通常pipenv ~は必要なし。  

- サイトのプレビュー

``` bash
pipenv run mkdocs serve
```

- サイトビルド

``` bash
pipenv run mkdocs build
```

- 標準のgithub.ioへのアップ  

``` bash
pipenv run mkdocs gh-deploy
```