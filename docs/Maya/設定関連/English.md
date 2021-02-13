# OSの言語に関係なく、別言語で利用する
## 概要
基本的にOSは日本語にしつつ、  Mayaは英語以外だと何かと問題になるので、変更方法  


## 前提環境
- Windows10
- Maya2020(mayapy2.7)

!!! Danger
    Maya2020から？言語が「日本語」と「英語、日本語、中国語」の全部入りが選べる様になっていました。  
    この変更で言語切り替えは全部入りを選択した時のみ反映される可能性があります。  

## MAYA_UI_LANGUAGEを変更する
### 英語
``` bash
set MAYA_UI_LANGUAGE=en_US
maya
```
### 日本語
``` bash
set MAYA_UI_LANGUAGE=ja_JP
maya
```
### 中国語
``` bash
set MAYA_UI_LANGUAGE=zh_CN
maya
```