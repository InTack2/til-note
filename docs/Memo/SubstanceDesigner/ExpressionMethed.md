# ノードでの表現方法
Substance Designerでこうやったらこうできる面白いっ！っと思ったものを書いていく。

## 傾斜を作る
[![Image from Gyazo](https://i.gyazo.com/be4d54391989624a08f9ec5ee9fefcb9.gif)](https://gyazo.com/be4d54391989624a08f9ec5ee9fefcb9)

## 境界線を滑らかにする
イメージとしては中央→外に向けて
[![Image from Gyazo](https://i.gyazo.com/472e15d0b38cc41457522ac3dc4507e7.png)](https://gyazo.com/472e15d0b38cc41457522ac3dc4507e7)


## アウトラインを抽出するその1
色々な方法で取れそうなので、その1  
Thresholdで取得する。  
[![Image from Gyazo](https://i.gyazo.com/a17fd32aacfb14ddf187080e3990033b.png)](https://gyazo.com/a17fd32aacfb14ddf187080e3990033b)  

## 一つのセル内ごとに影響範囲を変えたり色を変えたりする
Flood Fillとそれに関連するノードを使用する。  
[こちら](http://monsho.hatenablog.com/entry/2017/10/21/225521)が非常に参考になります。

## 形状を歪ませる
[![Image from Gyazo](https://i.gyazo.com/4370a78fe0258de0627d15e743a0dd76.png)](https://gyazo.com/4370a78fe0258de0627d15e743a0dd76)

## ある形状に欠けを追加する
何か規則的な建築パターンなどの輪郭に欠けを追加したりで便利そう。  

## それっぽい砂地を作る
[![Image from Gyazo](https://i.gyazo.com/c33f282c5a85f2568df4fbe12041fd54.png)](https://gyazo.com/c33f282c5a85f2568df4fbe12041fd54)


## 風を追加する
Directional scratchesを引き延ばし、VectorWarpで歪ませる  
そして、それをSlope情報としてSlope Blur Grayscaleに接続する。  
VectorMapは法線などでも可  

[![Image from Gyazo](https://i.gyazo.com/b6be1a0ac90fc7d3078e4350bfa5a6ea.gif)](https://gyazo.com/b6be1a0ac90fc7d3078e4350bfa5a6ea)

## 風その2(形状に合わせるならこちらが楽そう)
スクラッチで作った線にガウスノイズでワープで変形して少しランダム感を出す
そしてそれをVectorWarpで形状に添わせる。  
例えば頂上近くは必要ないなどあればHistogram　SelectなどでマスクしてNon uniform blurなどでブラーをかけて目立たなくさせる。
Non uniform blurを使わなくても普通にかけたくない所をマスクしても見た目に差異はなし。  

[![Image from Gyazo](https://i.gyazo.com/3156675ea553b9a8040f8d3fcf48599d.png)](https://gyazo.com/3156675ea553b9a8040f8d3fcf48599d)


## 地割れ模様を作る
Tile Randomでほとんど形状を作り、Edge Detectで境界線を抽出。  
Edge Detectの結果で境界線を出力できるので、かなり応用が効く。  

[![Image from Gyazo](https://i.gyazo.com/b0276a7cbf6ae02e829df704fd9589a1.png)](https://gyazo.com/b0276a7cbf6ae02e829df704fd9589a1)

## 地割れ模様にランダム感を足す
[![Image from Gyazo](https://i.gyazo.com/d2c48f0841db6ccfb51dfe75ebf11d5c.png)](https://gyazo.com/d2c48f0841db6ccfb51dfe75ebf11d5c)



