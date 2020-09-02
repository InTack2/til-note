# Github Actions
[Github actions](https://docs.github.com/ja/actions/getting-started-with-github-actions/about-github-actions)  
このサイトのビルドにも使用しているCI/CD  
今後OSSで公開などしたライブラリのドキュメントを表示などしたいので、先に用意  

## 感想
先にやってみた感想としては、  
ちょっとWindowsでやるのは、ローカルでテストしづらく  
構成が完成するまで、プッシュなどを重ねて確認する印象があった。  
publicリポジトリで完全にローカル構築できる方法があれば、印象が変わるかも..  
(private向けだったり、windowsではパスのエラーが起きたりといまいち上手くいかない状態だった。)

## 書き方
総じてkubernetesなどのもそうだが、構成をyamlで定義している物は  
公式ドキュメントの構文を見ながら一つずつ進めないと全然訳わかめなので[公式](https://docs.github.com/ja/actions/configuring-and-managing-workflows/configuring-a-workflow)をお勧めしたい。  
参考に、このサイトのビルドに使用している物を下記に記載。

!!! Tip
    ポイントはCI/CDは高速で回せないといけないので、cacheなども入れて高速化を意識している事。  
    Linuxのおかげもあるが、大体このJobでcacheが効いて40秒位

```YAML
name: MkDocs Deploy

on:
  push:
    branches: master
    paths:
      - "docs/**/*.md"
      - "mkdocs.yml"
jobs:
  deploy:
    runs-on: ubuntu-latest

    steps:
      # Gitのユーザー設定
      - name: Git config
        run: |
          git config --global user.name "github-actions"
          git config --global user.email actions@github.com

      # Git認証
      - name: Checkout
        uses: actions/checkout@v2

      # Pythonインストール
      - name: Set up Python
        uses: actions/setup-python@v2
        with:
          python-version: "3.8"

      # pipenvインストール
      - name: Install pipenv
        run: |
          pip install pipenv

      # pipenvのvirtualenvをcache
      - name: Cache pipenv virtualenv
        id: cache-pipenv
        uses: actions/cache@v1
        with:
          path: ~/.local/share/virtualenvs
          key: ${{ runner.os }}-pipenv-${{ hashFiles('**/Pipfile.lock') }}

      # cacheがなければ、インストール
      - name: Install dependencies
        if: steps.cache-pipenv.outputs.cache-hit != 'true'
        run: pipenv install

      # MkDocsのデプロイ
      - name: MkDocs deploy
        run: |
          pipenv run mkdocs gh-deploy

```

## act
[act](https://github.com/nektos/act)  
Github Actionをローカルで回す為のテストランナー  
dockerで環境立ててやるので、良さそうではあったのですが、ホストがWindowsだとパスのエラーが起きて上手くいかず  
WSL2だと上手くいったという話があるので、今度試してみるかも。