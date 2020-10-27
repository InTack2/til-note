# 既存ノードメモ

## Transform2D
変形。  
2DViewで変形もできる。  


## Edge Detect
[公式](https://docs.substance3d.com/sddoc/edge-detect-159450524.html)  

[![Image from Gyazo](https://i.gyazo.com/7d217aead640a6c46ebb891570971b92.png)](https://gyazo.com/7d217aead640a6c46ebb891570971b92)
境界線を検出する。

## Shape Splatter
[公式](https://docs.substance3d.com/sddoc/shape-splatter-172819773.html)  
形状を並べる。  
格子状に並べたり、色々ランダムに並べたり複雑なノード。  
[![Image from Gyazo](https://i.gyazo.com/94e0d1abe95a63f434dc6721d20e8e26.png)](https://gyazo.com/94e0d1abe95a63f434dc6721d20e8e26)  


## Auto Levels
[公式](https://docs.substance3d.com/sddoc/auto-levels-159449154.html)
コントラストを白黒の最大値になる様に、調整するノード。  
ハイコントラスト化というイメージがわかりやすそう。  

[![Image from Gyazo](https://i.gyazo.com/8317f0c97e3fcae75e1212419e44e5b5.png)](https://gyazo.com/8317f0c97e3fcae75e1212419e44e5b5)


## Non Uniform Blur
[公式](https://docs.substance3d.com/sddoc/non-uniform-blur-159450461.html)
ぼかし。  
画像では同じノードを接続して、外側になるにつれてボケる→滑らかにしている。  
[![Image from Gyazo](https://i.gyazo.com/472e15d0b38cc41457522ac3dc4507e7.png)](https://gyazo.com/472e15d0b38cc41457522ac3dc4507e7)


## Gradient Linear
ただのリニアグラデーションマップだが、HightMapを作るシェープとBlendで組み合わせると面白い事になる。  
[![Image from Gyazo](https://i.gyazo.com/be4d54391989624a08f9ec5ee9fefcb9.gif)](https://gyazo.com/be4d54391989624a08f9ec5ee9fefcb9)
そのグラデーションの向きに合わせて「傾斜」にすることができる  

## Blur HQ Grayscale
[公式](https://docs.substance3d.com/sddoc/blur-hq-159450455.html)
ぼかしだが、ハイトマップで薄くかけることで境界線のジャギ付きをとったりできる  


## Threshold
[公式](https://docs.substance3d.com/sddoc/threshold-200574221.html)
ヒストグラムスキャンより精度高く実行できる。コントラストは最大

## Directional Warp
指向性ワープ。srcをdistのマップで移動させる処理となるが、  
ハイトマップにグランジなどのマップを薄くかけることでパターンを自然にゆがませる形に使用。  
[![Image from Gyazo](https://i.gyazo.com/ffda8c15bcbb26e970b5a9f795a41fd5.png)](https://gyazo.com/ffda8c15bcbb26e970b5a9f795a41fd5)

## Slope Blur Grayscale
[公式](https://docs.substance3d.com/sddoc/slope-blur-159450467.html)  
上のワープの様な形で使う。  
ハイトではイメージとしては輪郭が欠ける様な表現で使用される  


## Grunge_map_001
コンクリートのひび割れっぽい

## safe transform Grayscale
まだ調べてない

00:48:45でストップ
https://www.twitch.tv/videos/773517254?t=0h48m45s