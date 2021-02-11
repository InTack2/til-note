# みんなのBlender LT会

## 概要
みんなのBlender用で作った物のシーン解説と軽いTips  

## 内容
こんな感じのオーディオによって動く光  
[![](http://img.youtube.com/vi/zOb8eaq3654/0.jpg)](http://www.youtube.com/watch?v=zOb8eaq3654 "minble")  

## 配布
下記のリンクからダウンロード可能。  
[ダウンロードリンク](https://drive.google.com/file/d/13WFu-RmZCnacIkcXO2Q-Gzmj04pb1MjV/view?usp=sharing)  

## シーン解説

### IBL(イメージベースドライティング)

画像の輝度、色をライトとして利用する手法  
反射やライティングを簡単にいい感じにできる。

HDRIは撮影するのは大変なので、フリーの物を感謝しながらダウンロード。  
[https://hdrihaven.com/](https://hdrihaven.com/)  

[![Image from Gyazo](https://i.gyazo.com/402f4bfa172b1f10b5fcea750208fb6c.png)](https://gyazo.com/402f4bfa172b1f10b5fcea750208fb6c)  

### 構成
[![Image from Gyazo](https://i.gyazo.com/35e24c132fed3fde878028265d6fcb29.png)](https://gyazo.com/35e24c132fed3fde878028265d6fcb29)  

元モデルが電球2つの大本のモデル。  
LightBulb001と002がインスタンス複製？Blenderでの名前がわからないので、あれですが..  
元モデル一つで設定すると001と002に適用されてる状態。  
Mayaの様にグループ化ができない？様なので、疑似的にコレクションでまとまりを作ってます。  

#### 床
##### マテリアル
粗さを下げると反射する。  
反射すると電球が綺麗に見えるので、かなりつるつるに  
[![Image from Gyazo](https://i.gyazo.com/54aba3a7ee2253f2c3b058fc6765e966.png)](https://gyazo.com/54aba3a7ee2253f2c3b058fc6765e966)  

#### 電球
##### マテリアル
ガラスの材質を適用。
作り方は下記の動画が参考になります。  
基本的にIORと伝播辺りとアルファで調整というイメージ。  
[【Blender初心者講座】グラス作る 後編/CG制作の基本](https://www.youtube.com/watch?v=HmxV6fppqfA)  
[![Image from Gyazo](https://i.gyazo.com/404fbe97323252c51509f551abe39b7a.png)](https://gyazo.com/404fbe97323252c51509f551abe39b7a)  

後は口金ですが、スタンダードな金属のPBRマテリアルなので、割愛  


##### 音に合わせて動くフィラメント
こんなイメージ。  
[![Image from Gyazo](https://i.gyazo.com/a07411d7a6253d990ef28e78e5222c65.gif)](https://gyazo.com/a07411d7a6253d990ef28e78e5222c65)  

音とモデルの連携方法を下記を参考に  
今回純粋にZ軸のスケールを音と同期させてます。  
[【Blender】音に合わせてオブジェクトを動かす方法](http://rikoubou.hatenablog.com/entry/2018/05/02/153651)  

モデルは実はただのキューブ一つでモデファイヤーや複数配置、Curveに沿って位置を変更させてます。
(なので、少し角度がついた状態でスケールされている。)  

##### ワールドポジションマッピングテクスチャ
こんな感じ。  
[![Image from Gyazo](https://i.gyazo.com/3139cef4723af00957cf3f6a7a6a2d43.gif)](https://gyazo.com/3139cef4723af00957cf3f6a7a6a2d43)  

[![Image from Gyazo](https://i.gyazo.com/40fc8c4761cfff718cabb491be6a8933.png)](https://gyazo.com/40fc8c4761cfff718cabb491be6a8933)  

この方式のよい所はモデファイヤなどで今後ランダム配置等した時に位置で色が変えられる点。  
後は今は奥方向で色が変わる様にしているが、上方向で色が変わる様にするとスケールしたフィラメントの色を変えたりもできる。  

方法としては、下記の様な画像を用意  
[![Image from Gyazo](https://i.gyazo.com/c4c3fd22a7eb8c3db62c5076e553e2e9.png)](https://gyazo.com/c4c3fd22a7eb8c3db62c5076e553e2e9)  

そのテクスチャのUV情報は座標を0-1で持ってるので、この位置を頂点のモデルの位置と重ねる。  
こうする事でモデルの位置が変わるとそのUVの位置も変わるので、色が変わるという形となる。  
下記の画像の様に緑色のY軸とテクスチャの位置が同期しているイメージ。  
[![Image from Gyazo](https://i.gyazo.com/af9739c6115420878fdeb9b438ef4c69.png)](https://gyazo.com/af9739c6115420878fdeb9b438ef4c69)  
