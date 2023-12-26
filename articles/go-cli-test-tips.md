---
title: "GoでサクッとCLIツールを作る時にテスタブルにするtips"
emoji: "🏭"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["go", "cli"]
published: true
---

GoでCLIツールを作る際のテストしやすい設計がわからなかったので、現状調べた感じの良さそうな実装をメモします。

## CLIの構造体を作成する

```go:main.go

// io.Writerで出力先の構造体を宣言
type CLI struct {
  outStream, errStream io.Writer
}

// 実行部分をRunメソッドとして定義しておいて、コマンド実行時の引数などをメソッドの引数として入れる
func main() {
  cli := &CLI{outStream: os.Stdout, errStream: os.Stderr}
  // 戻り値としてステータスコードをリターンする
  status := cli.Run(os.Args)
  os.Exit(status)
}

```

## なぜio.Writerなのか
io.WriterはWriteメソッドを必要とするinterface型です。

```go
type Writer interface {
  Write(p []byte) (n int, err error)
}
```
出典：https://pkg.go.dev/io#Writer

write methodはbyteを受け取ってデータとエラーを返すらしいので、このメソッドがあればこのinterfaceを満たすことにもなり、範囲としてはとても大きい型（だと思う）。とりあえず何か入出力があれば使えるので、これを利用して出力先の制御をします。

## テストではバイト列を使用する
標準出力ではなく、バイト列に書き込むようにすることで、出力先を制御することができます。
これによって出力内容のテストが楽になります。

```go
// test実行前に実行する関数
func setup_test(a string) (*CLI, []string, *bytes.Buffer, *bytes.Buffer) {
  outStream, errStream := new(bytes.Buffer), new(bytes.Buffer)
  cli := &CLI{outStream: outStream, errStream: errStream}
  args := strings.Split(a, " ")
  return cli, args, outStream, errStream
}
```

## 参考
https://gihyo.jp/article/2022/11/tsukinami-go-03
