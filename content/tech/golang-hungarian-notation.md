+++
draft = false
title = "Go言語でアプリケーション・ハンガリアン記法的なことを型で表現する"
tags = ['go']
date = "2017-04-03T17:35:08+09:00"
+++

[間違ったコードは間違って見えるようにする](http://local.joelonsoftware.com/wiki/%E9%96%93%E9%81%95%E3%81%A3%E3%81%9F%E3%82%B3%E3%83%BC%E3%83%89%E3%81%AF%E9%96%93%E9%81%95%E3%81%A3%E3%81%A6%E8%A6%8B%E3%81%88%E3%82%8B%E3%82%88%E3%81%86%E3%81%AB%E3%81%99%E3%82%8B)
で説明されている「アプリケーションハンガリアン記法」とは、

- 同じデータ型の変数を区別するために、内容を示す名前をつける

というものです。
先のページで挙げられている例では、行と列の最大値に `rowMax` `colMax` と名前をつけることで取り違えを防ぐ、といった感じです。
これらはともに整数なので相互に代入できてしまいますが、バグなので、命名規則で気づきやすくする、という意図です。

Go 言語では型に別名をつけることができ、コンパイル時の型チェックで違う型として扱われるため、取り違えを防ぐのに有効です。

<!--more-->

例えば緯度・経度などはともに実数ですが、それぞれ別の型名を与えておくと型チェックを利用できてうれしいです。

{{< gist tmrtmhr ae1025314c09b102d43189e6232a1cd7 >}}
