---
title: "EAS build, submit, update周りを理解する"
emoji: "🚝"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["expo", "reactnative"]
published: false
---

expo + react nativeで開発していると、EAS周りの理解は不可欠です。
今回久しぶりにreact nativeを触ったこともあって、全体像をキャッチアップするのが時間がかかってしまいました。

ざっくりの全体像を理解するためのメモとして記載します。

## EAS build
EAS buildはアプリのバイナリを作成する仕組み、およびexpoのホステッドサービスにおける該当機能のことを指します。
コマンドからバイナリを生成し、expo（ホステッドサービス）にアップロードします。

storeに提出する際、ここで作ったバイナリを使用します。

## EAS submit
EAS submitはEAS buildで作成したアプリのバイナリをPlay storeやapple storeにアップロードする仕組みを指します。
コマンドから作成されたバイナリを指定して、各storeにアップロードしてくれます。

また、`eas build`実行時に`--auto-submit`を使用することで、ビルドから自動でstoreにアップロードさせることができます。

## EAS update
EAS updateはネイティブレイヤーを除いた箇所である、JS部分をstoreを介さずともアップデートできる仕組みです。
「storeを介さずとも」と記載しましたが、これはinternal distributionなどstaging環境のアプリにも適用できます。

つまり、EAS updateを用いれば、バイナリを作成する必要のない変更に限っては、サクッとアップデートを適用できるというメリットがあります。（storeに提出するのがやっぱり地味に一番めんどい）

このEAS updateの概念のページはぜひみてください。
https://docs.expo.dev/eas-update/how-it-works/

## eas.json
EASの中で難しいのがprofileとchannelとbranchの違いだと思います。
ここを理解しているとドキュメントの理解も進み、実際にeas.jsonで設定もしやすくなると思います。

### profile
profileとは`build`キーや`submit`キー配下に設定する名前付きのパラメータなどの構成グループです。
`--profile <profile-name> `でコマンドから指定することができ、指定しなかった場合はproductionをデフォルトとして使用します。(https://docs.expo.dev/build/eas-json/#build-profiles)

### channel
channelはprofileで設定できる項目で、channelを指定してバイナリを作成したりすることができます。profileと同様、更新するラインを作ることが可能。（profileにおける）

想像ですが、profileにぶら下げてPRごとにchannelを作りchannelごとの環境を立ち上げたりできそう。

### branch
branchはその名の通りでgitのbranchです。EASにおいては、channelはマッチするbranchと自動でリンクされています。

これを利用して、特定のブランチの更新をリンクしているchannelに適用させたりすることができたりします。（https://docs.expo.dev/eas-update/how-it-works/#matching-updates-and-builds）

## まとめ
ざっくりの概要ではありますが書きました。
EASは便利なので、全体を理解しつつカスタムしていくと開発効率も爆上がりです！

### 参考
https://docs.expo.dev/build/introduction/
https://docs.expo.dev/submit/introduction/
https://docs.expo.dev/eas-update/introduction/


