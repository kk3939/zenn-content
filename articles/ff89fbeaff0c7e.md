---
title: "puppeteer + Jest でtestが完了してもexitしなかった"
emoji: "🗂"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: [jest, puppeteer]
published: false
---

## 起きたこと
puppeteerを使うライブラリを開発している際に、jestを使ったtestで下記のエラー?warning?が出た。

```
Jest did not exit one second after the test run has completed.
This usually means that there are asynchronous operations that weren't stopped in your tests. Consider running Jest with `--detectOpenHandles` to troubleshoot this issue.
```

testは正常に完了していたので、原因特定に時間を少しかけてしまった。

## 原因
公式のドキュメントに答えを発見。
ドキュメントのGuidesの部分に丁寧にpuppeteerを使う際の注意事項が記載されている。

https://jestjs.io/docs/puppeteer#custom-example-without-jest-puppeteer-preset

puppeteerでbrowserやpageをcloseしないとprocessが残ってしまい、上記エラーが出る模様。
`await page.close();`や`await browser.close();`を忘れないようにすれば**OK.**

## 参考にしたサイト

https://oisham.hatenablog.com/entry/2018/12/11/233231

https://jestjs.io/docs/puppeteer#custom-example-without-jest-puppeteer-preset



