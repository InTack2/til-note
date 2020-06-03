# MkDocs
このサイト自体に使用している静的サイトジェネレーター。  


## 基本コマンド
個人的にpipenv使用なので、pipenv ~を記載してますが、通常pipenv ~は必要なし。  

- サイトのプレビュー
```bash
pipenv run mkdocs serve
```

- サイトビルド

```bash
pipenv run mkdocs build
```

- 標準のgithub.ioへのアップ  
下記の理由により使用していない
    - 個人的にブランチ分けるほどでもない
    - github.ioが指定フォルダをサイトにアップできる様になった

```bash
pipenv run mkdocs gh-deploy
```