+++
draft = false
title = "AWS Lambda と Fargate のCPU時間単価および性能比較"
tags = ["aws"]
date = "2018-09-29T00:47:50+09:00"
+++

東京リージョンでAWSのマネージドコンテナサービス [Fargateの料金](https://aws.amazon.com/jp/fargate/pricing/)を見ると
1 vCPU: $0.0632/h, 2GB メモリ: $0.0316/h で、合わせて $0.0948/h です。
一方[Lambdaの料金](https://aws.amazon.com/jp/lambda/pricing/)は 2GB メモリ: $0.120024/h です。

あれ、なんか Fargate 安くないですか？

本記事はこの料金の違いがどういう理由によるものかの筆者の推測、
および Lambda と Fargate の処理性能の比較について述べるものです。

結論をまず言うと以下のようになります。

- 1 vCPU 2GB の Fargate と 2GB の Lambda であれば Fargate のほうが速い
- Fargate のほうが安いのはAPIサーバなどの用途で冗長性を持たせようとすると CPU を100%使えないから

<!--more-->

# Lambda と Fargate の速度比較

2GBメモリの Lambda が vCPU いくつに相当するかは明記されていないので、
まずざっくり計測してみます。

ソースコードは以下の通りです(Lambda, Fargate 共通)。
フィボナッチ数列の40番目を印字するだけです。
Lambda では引数を渡すために別モジュールとして読み込まれて `lambda_fuction.lambda_handler`のように呼ばれるので
`if __name__ == "__main__":` のくだりは実行されません。
処理系は python 3.6 です。

```
import time

def fib(n):
    return n if n < 2 else fib(n-1) + fib(n-2)

def lambda_handler(event, context):
    start = time.time()
    print(fib(40))
    end = time.time()
    print(f"elapsed time: {end-start}")
    return None

if __name__ == "__main__":
    lambda_handler({}, {})
```

Dockerfile は以下の通りです。
```
FROM python:3.6.6-alpine

WORKDIR /app
ADD . /app

CMD [ "python", "lambda_function.py" ]
```

実行時間は以下の通りです。一回しか計測していません。

|計算リソース|実行時間(秒)|
|---|---|
|Lambda 1024MB|84.39|
|Lambda 2048MB|51.39|
|Lambda 3008MB|51.57|
|Fargate 1vCPU 2GB|40.42|

何ということでしょう…… Fargate のほうが速いではありませんか。
時間単価が安いにも関わらず……。

(本記事の本筋とは外れますが Lambda 2048MB と 3008 MB で顕著な差が見られないのも気になります)

この違いはどういった理由によるものでしょうか？
自分の推測では、鍵は冗長性にあります。


# N+1 ルール (N+1冗長)

VMやコンテナを使った場合のAPIサーバの冗長化を考えましょう。
1台では落ちたら終わりなので最低2台は必要です。

では2台ともCPU負荷100%まで酷使するべきでしょうか？

2台ともCPU負荷100%の状態で1台がダウンしたときのことを想像してみましょう。
残った1台には200%分の負荷がかかります(退職した人の仕事が自分に回ってきたときと同じです)。
処理しきれないリクエストが溜まり続けて、残った1台もダウンしてしまうでしょう。

冗長化のためには、1台がダウンしたとしても、残ったリソースの負荷が100%を超えないように余力を残す必要があります。
2台の場合には50%、3台の場合には66%、4台の場合には75%まで……というふうに負荷の上限が決まります。
負荷100%の状態から1台追加(N+1)して余力を残すことによって冗長性を確保するわけです。

これは、冗長性のためにCPUを100%使いきることができないということです。
クラウドサービスのオートスケール設定でも、「平均CPU負荷が80%を超えたら1台追加する」のようなルールになるため、
100%に近づけることはせず、設定した負荷付近で運用することになります。


# 考察

計測した実行時間からすると、Lambda 2GB の処理性能は Fargate 1vCPU の80%程度のように見えます。
ただしこの実行時間は両者ともにCPUを全力で使った場合のものです。

Fargate をAPIサーバとして利用し、冗長性のため80%付近で運用するならば、
Lambda の時間単価と同等ということになるでしょう。

Lambda は冗長性を考慮しなくともよいため常にCPU100%使えるのに対し、
Fargate は冗長性のために100%使えない……ということが料金に反映されているのだと考えています。

言い換えれば、SQSに追加されるタスクを処理していくような常にCPU100%使ってよい場面であれば、
Fargate のほうが安いということになります。
Lambda は起動が早く、スパイクにも対応できるスケーラビリティが魅力だったりするので、
一概に安さだけで選ぶものでもないですが、
参考になれば幸いです。

(余談になりますが、性能当たりのコストで言うと EC2 m5.large (2 vCPU)が $0.124/h で、
1 vCPU あたり $0.062/h となるので、Fargate よりさらに安いです。
おまけにスポットインスタンスやリザーブドインスタンスも利用できるのでEC2も捨てたものではないです。)

# まとめ

- Fargate の料金は冗長性のためCPUを100%使いきれないユースケースを想定したもの(個人の感想です)
- Lambda 2GB のCPU性能は Fargate 1 vCPU の 80 %