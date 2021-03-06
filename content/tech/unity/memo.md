+++
date = "2018-02-19T11:13:43+09:00"
draft = true
title = "Unity メモ"
tags = ["Unity"]
+++

<!--more-->

# Button にスクリプトをひもづける

https://qiita.com/2dgames_jp/items/b3d7d204895d67742d0c

1. Hierarchy の Button にスクリプトを D&D (Button の Component にスクリプトを追加)
1. Button の Inspector > On Click の項目を設定
   このとき追加するのは先ほど設定したボタン GameObject。

# Scroll View

https://qiita.com/okuhiiro/items/14b829778d226bc2f2ed

【Unity】uGUIよりも手前に3Dオブジェクトを配置したUIを作る
http://megumisoft.hatenablog.com/entry/2016/01/27/235940

# Tango で 3d モデルが真っ黒になる
Shader の設定を Tango / Environmental Lighting / Diffuse に設定

# emacs C# mode

http://blog.tkeo.info/blog/2014/12/10/unity-with-emacs/

https://note.nkmk.me/c-sharp-mac-atom-omnisharp/

#
UnityでUIを消したスクリーンショットを撮る方法
https://qiita.com/SARU_KABUTO/items/5a7d5178797614df896b

# Unity でボタンを押しても反応がない

https://qiita.com/otochan/items/abcbb2cc5a8a5ea79516
* EventSystem がSceneに追加されてない
* 透明なオブジェクトに当たっている

# Input.touch が uGUI と競合する場合

Unityで、タップされた位置がボタンの上だったら反応させない
https://tyfkda.github.io/blog/2016/11/13/unity-tap-ui.html

【Unity】スクリプトからPrefabのインスタンスを作る方法
https://qiita.com/2dgames_jp/items/8a28fd9cf625681faf87


PrefabとInstantiateの基本（1）ブロックをScriptで配置：「はじめてのUnity」のブロック崩しを改造しながら学ぶ
https://qiita.com/JunShimura/items/7e45fc6236cf97914041

[Unity]他のオブジェクトについているスクリプトの変数を参照したり関数を実行したりする。
https://qiita.com/tsukasa_wear_parker/items/09d4bcc5af3556b9bb3a

親オブジェクトを取得
GameObject  parent =  gameobject.transform.parent.gameObject;

Tangoで現実の床に仮想物体の影を落とす
http://bibinbaleo.hatenablog.com/entry/2017/07/12/211715

Tango で Occulusion を有効にする
TangoManager > Enable Depth ON
TangoCamera > Enable Occulusion ON
TangoPointCloud > Update Point Mesh ON
Build Setting > Player Setting > Auto Graphics API Off, OpenGLES 2.0 を優先
https://qiita.com/shiroizu/items/6bb3eea3ddaa41bd61d5
一からTangoを始めて詰まったこと
https://qiita.com/blkcatman/items/63133b3a4eeea416d61a

Tango Dynamic Meshでキャラクターの移動を制限する
https://qiita.com/jyuko/items/0735a6cc3971750de0ff

[Unity] uGUIとUniRxで四方向へのスワイプを実装する
https://qiita.com/lycoris102/items/1c792c4ba78e564a1b21

# 3DモデルがRaycastに反応しない場合
3DモデルのInspectorでGenerateColliderにチェック

# Tango で DynamicMesh を追加
TangoManager で Enable 3D Reconstraction を ON
サンプルの DynamicMesh GameObject をコピーして Scene に追加する

[UnityでUIを消したスクリーンショットを撮る方法](https://qiita.com/SARU_KABUTO/items/5a7d5178797614df896b)

[yield return null と WaitForEndOfFrame の違い](http://robamemo.hatenablog.com/entry/2017/09/13/111843)
[【Unityシェーダ入門】円やリングをかっこよく動かす方法](http://nn-hokuson.hatenablog.com/entry/2016/11/14/203745)

[floatの値が0〜1の範囲内に収まっているかif文を使わずに判断する](http://ina-amagami.hatenablog.jp/entry/2016/10/06/221454)
[Render Textureでカメラの映像をスクリーンに映す](http://mikasa.hatenablog.jp/entry/2015/05/05/051008)
[Tango Dynamic Meshでキャラクターの移動を制限する](https://qiita.com/jyuko/items/0735a6cc3971750de0ff)

[Google Tango UnityのNavMeshで経路表示](https://www.youtube.com/watch?v=CRRIO4J-X_E)

# 3D Text Occulusion
http://wiki.unity3d.com/index.php?title=3DText

[What kind of precision can the Tango acheive?](https://stackoverflow.com/questions/34672723/what-kind-of-precision-can-the-tango-acheive)

https://developers.google.com/tango/overview/depth-perception?hl=ja
0.5m-4m
必要電力と精度のバランスのため。
赤外線を使うため、太陽光や白熱球の影響を受ける。
および色の黒い赤外線を反射しない物体に対する使用は正確に測定できないため推奨されません。

[やはりUNITYでAndroidプラグインを作るにはAndroidJavaClassが便利だ](https://qiita.com/YukiMiyatake/items/c8c2ef396fbf4457ba4f)

[Android開発 | 画像のExif属性に位置情報や撮影日時を保存する方法](http://mussyu1204.myhome.cx/wordpress/it/?p=161)
