# cookiecutterでMaya用のテンプレートを作る

## きっかけ
最近SubstanceDesignerのプラグイン開発を試している時に
ドキュメントに「テンプレートはcookiecutterを利用している」と記載があり、  
調べてみると[Django](https://github.com/pydanny/cookiecutter-django)や[Flask](https://github.com/cookiecutter-flask/cookiecutter-flask)なども出しており、Githubで検索するとかなりの量が[ヒット](https://github.com/search?q=cookiecutter)します。  

以前個人的に「[boip](https://github.com/InTack2/boip)」という置き換えモジュールを作っていたのですが、それよりも断然使いやすそうだぞっ！という事で検証し、Maya用にサンプルを作成してみました。


## 概要
<a href="https://github.com/InTack2/cookiecutter-mayaqt-mvc"><img src="https://github-link-card.s3.ap-northeast-1.amazonaws.com/InTack2/cookiecutter-mayaqt-mvc.png" width="460px"></a>  

cmdから実行し、質問に答えると下記のようなMaya用のテンプレートコードを生成することができます。  
[![Image from Gyazo](https://i.gyazo.com/678b5e203f8e8917f72d56bad9507c04.png)](https://gyazo.com/678b5e203f8e8917f72d56bad9507c04)[![Image from Gyazo](https://i.gyazo.com/a9faef8a5930e4eebaa5e3765ae9673b.png)](https://gyazo.com/a9faef8a5930e4eebaa5e3765ae9673b)

## 導入方法
### cookiecutterのインストール
`python -m pip istall cookiecutter`

### 上記のテンプレートを実行
`python -m cookiecutter git+https://github.com/InTack2/cookiecutter-mayaqt-mvc.git`


## 構成
大枠の使い方は、リポジトリを見ていただく事で把握可能かと思いますが、  
簡単にテンプレートの作り方を説明したいと思います。  
[公式](https://cookiecutter.readthedocs.io/en/1.7.2/README.html)

### cookiecutter.jsonを用意する
cookiecutterの設定、質問内容をこちらに記載します。  
```json
{
    // 質問内容
    "full_name": "Your name",
    "email": "Your address email (eg. you@example.com)",
    "tool_name": "Name of the project",
    "tool_slug": "{{ cookiecutter.tool_name|lower|replace(' ', '-') }}",
    "release_date": "{% now 'local' %}",
    "version": "0.1.0",
    
    // jinja2の拡張機能利用
    "_extensions": [
        "jinja2_time.TimeExtension"
    ],
    "_copy_without_render": [
        "cookiecutter.json",
        "{{cookiecutter.tool_slug}}"
    ]
}
```
`"full_name": "Your name"`などが一つずつ質問されます。  

### 質問の答えの置き換え先を用意する
ユーザーが質問に対して入力した結果が`{{cookiecutter.質問名}}`として記載されたフォルダ名、ファイル名、コード内を置き換えを行います。  
上記の例だと`{{cookiecutter.full_name}}`ですね。  

私が用意したサンプルだと下記の様に`tool_name`でフォルダ名を指定する形にしています。  
```
{{cookiecutter.tool_name}}
  ├{{cookiecutter.tool_name}}
  │└ツールの中身
  └LICENSEなどなど
```

PythonのDjangoやFlaskを使用された事ある方はピンっと来たかもしれませんが、お察しの通りjinja2を利用し置き換えを行っている様です。  

### 一部文字の置き換えや小文字などに指定する
上記の質問内容の`tool_slug`部分で利用しています。  
少し記載方法が特殊な感じですが、意味としては、tool_nameの答えを小文字(lower)にし、`' '`半角スペースを`-`ハイフンに上書きするといった感じです。  
```json
    "tool_slug": "{{ cookiecutter.tool_name|lower|replace(' ', '-') }}",
```

### Jinja2の拡張機能を利用する
サンプルでも現在の年月日取得用に利用しています。  
本来は`pip install`等で追加して利用しますが、公式で下記の3つはインストール済みな様です。  
`cookiecutter.extensions.JsonifyExtension`  
`cookiecutter.extensions.RandomStringExtension`  
`jinja2_time.TimeExtension`  

```json
    "release_date": "{% now 'local' %}",
    "version": "0.1.0",
    
    // jinja2の拡張機能利用
    "_extensions": [
        "jinja2_time.TimeExtension"
    ],
```
### 選択式にする
質問内容を配列にする事で選択式にすることができます。  
ただ、選択した結果をjinja2の様にifで区切る事になるので、かなりifif祭りになりそうな予感がして微妙な気もしますが。  

```json
{
    "license": ["MIT", "BSD-3", "GNU GPL v3.0", "Apache Software License 2.0"]
}
```
詳しくは[こちら](https://cookiecutter.readthedocs.io/en/1.7.2/advanced/choice_variables.html)から

### 前処理、後処理を追加する
サンプルでは特に必要なかったので、利用しませんでしたが、  
置き換え前、置き換え後に「hook」フォルダ以下のpythonファイルで実行可能な様です。  
詳しくは[こちら](https://cookiecutter.readthedocs.io/en/1.7.2/advanced/hooks.html)から


### 個人的に気になる所
- 選択式の結果でテンプレート先を標準では変更できない。
    - hookを利用して自前でテンプレートフォルダを分ける
    - それぞれをリポジトリ分けて、指定した後にそのリポジトリを利用する形にするなど


以上です。  
サンプルも少し学習がてら作り、ツッコミどころ満載だと思いますので、  
こうした方が良いよ！などPR受け付けておりますので、お気軽にどうぞ！  

### おまけ

#### cookiecutterのテンプレート生成用のテンプレート
https://github.com/eviweb/cookiecutter-template
https://github.com/sourcery-ai/python-best-practices-cookiecutter
