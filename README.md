lamb - a simple deploy and test tools for aws lambda
=====

= これはまだアイデア段階

まだ、試せていないのでアイデア段階です。
目的はnodejs上のStreamベースのコードを自動的に必要な分だけのaws lambdaを生成し、
deployすることでアプリケーション全体のつながりをわかりやすく宣言的に記述できるようにします。


```
var lamb = require("lamb");
var s3 = lamb.s3;

s3("bucket-full")
    .pipe(onlyJpeg)
    .pipe(resize("10%"))
    .pipe(s3("bucket-mini"));

s3("bucket-full")
    .pipe(onlyJpeg)
    .pipe(resize("50%"))
    .pipe(s3("bucket-half"));
```

このようなコードを書いたときに、自動的に
それぞれの処理をAWS lambdaにdeployする。

```
$ lamb deploy index.js
```

方法としては、
それぞれのawsに関係するReadableStreamのコンストラクションを
記録し、イベント発火のコードを自動生成してzipに固め、lambda functionとしてuploadする

自動生成されるコードはこんな感じ。
```
var __ = require("index.js");
var handler = require("lamb").handler;
module.exports.handler = handler("s3:ObjectCreated:*");

```
また、テスト時にはnode.jsアプリケーションとして起動する

```
# lamb test
```



