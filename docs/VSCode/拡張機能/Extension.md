# 開発する

開発に使う機能や書き方メモ

## package.json

拡張機能のバージョンやカテゴリ名等の拡張機能の設定  
実際に作成したvscodeから呼び出せるコマンドを登録したりする。  
かなり重要。

※必要な設定を記載予定

### 1.拡張機能の情報設定

``` json
"name": "拡張名", //displayNameとの違いは確認中。
"displayName": "表示名",
"description": "説明",
"version": "0.0.1",
"engines": {
    "vscode": "^1.45.0"
},
"categories": [
    "Other"
],
```

### 2.拡張機能が有効になるコマンド設定

基本的に単体で使用するコマンドは登録しておくのが定石。  

例)
pythonの言語読み込みと該当の機能を呼び出したとき

``` json
    "activationEvents": [
        "onLanguage:python",
        "onCommand:登録した機能",
    ],
```

### 3.コマンド登録

``` json
"contributes": {
        "commands": [ //コマンドパレットで表示されるコマンド
            {
                "command": "呼び出されるコマンド",
                "title": "ユーザーが入力した時に表示される"
            }
        ],
        "menus": { //メニュー関連
            "explorer/context": [ //右クリックで表示される
                {
                    "group": "navigation@-1",　// Group設定
                    "command": "表示させるコマンド名"
                }
            ]
        }
    }
```

## メモ

良く使用する書き方等

### vscode関連のインポート

``` typescript
import * as vscode from 'vscode';
```

### 拡張有効化

その拡張機能がアクティブになった時に1度だけ呼び出される。  
アクティブになったかどうかは後記のtsconfig.json内で  
activationEventsに登録してあるコマンドが実行されたどうか  
コマンドと対応する関数登録に使用。  

``` typescript
export function activate(context: vscode.ExtensionContext){};
```

### コマンド登録

先ほどのactivate内に記載されるコマンド登録
register~~で種類あり。
※他の使い方確認中。

``` typescript

let disposable = vscode.commands.registerCommand('登録するコマンド名', sample);
context.subscriptions.push(disposable);

function sample() {
    vscode.window.showInformationMessage('Create!!');
}
```