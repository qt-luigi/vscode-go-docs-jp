# 本リポジトリーについて

Go言語のサポートを提供するVisual Studio Codeの拡張機能 [Microsoft/vscode-go](https://github.com/Microsoft/vscode-go) のドキュメントを日本語に翻訳してみました。
Google翻訳を使用して翻訳した後に自分でチェックして修正しています。
翻訳時のvscode-goのバージョンは 0.6.52 で、翻訳元のcommitは [ddb434c](https://github.com/Microsoft/vscode-go/blob/ddb434c0bf96771a0dc6f92d5e95ee737571154f/README.md) です。

# Go for Visual Studio Code

[![Join the chat at https://gitter.im/Microsoft/vscode-go](https://badges.gitter.im/Join%20Chat.svg)](https://gitter.im/Microsoft/vscode-go?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge&utm_content=badge) [![Build Status](https://travis-ci.org/Microsoft/vscode-go.svg?branch=master)](https://travis-ci.org/Microsoft/vscode-go)


**重要: 最近Go 1.7にアップグレードした場合、Goの補完リストが正常に動作するように端末で `gocode close` を実行する必要があります。 https://github.com/Microsoft/vscode-go/issues/441 を参照してください。**

[リリースノート](https://github.com/Microsoft/vscode-go/wiki/Release-Notes)を読んで、この拡張機能の直近のバージョンで何が変更されたのか確認してください。

この拡張機能はGo言語のための豊富な言語サポートをVS Codeに追加します。
含まれているもの:

- 補完リスト (`gocode` を使用)
- シグネチャーヘルプ (`gogetdoc` または `godef`+`godoc` を使用)
- スニペット
- クイック情報 (`gogetdoc` または `godef`+`godoc` を使用)
- 定義への移動 (`gogetdoc` または `godef`+`godoc` を使用)
- リファレンス検索 (`guru` を使用)
- ファイルの概要 (`go-outline` を使用)
- ワークスペースシンボルサーチ (`go-symbols` を使用)
- リネーム (`gorename` を使用。 注意: Windowsで名前を変更した後に元に戻すには、パスに `diff` ツールがあることが必要です)
- 保存時にビルド (`go build` と `go test` を使用)
- 保存時にlint (`golint` または `gometalinter` を使用)
- フォーマット (`goreturns` または `goimports` または `gofmt` を使用)
- ユニットテストのスケルトン生成 (`gotests` を使用)
- import文の追加 (`gopkgs` を使用)
- [_部分的に実装_] デバッグ (`delve` を使用)

### IDE機能
![IDE](http://i.giphy.com/xTiTndDHV3GeIy6aNa.gif)

## 使用方法

はじめに、Visual Studio Codeをインストールする必要があります。
つぎに、コマンドパレット (`cmd-shift-p`) で `Install Extension` を選択し、`Go` を選択します。

GOPATH環境変数が作業したいGOPATHに設定されている端末ウィンドウで、`code` を起動します。
GOPATHフォルダーまたは作業したいサブフォルダーを開き、編集を開始するために `.go` ファイルを開きます。
右下に `Analysis Tools Missing` の表示を確認し、これをクリックすると拡張機能が全機能セットをサポートするために必要なGoツールをすべてインストールします。
詳細については以下の [ツール](＃tools) セクションを参照してください。

_注意_: この拡張機能を使用する場合、Visual Studio Codeで `Auto Save` を有効にすることを検討する余地があります (`"files.autoSave": "afterDelay"`)。
Goツールの多くは保存されたファイルに対してのみ機能し、エラー報告は `Auto Save` が有効であることでよりインタラクティブになります。
`Auto Save` を有効にする場合、保存時にフォーマットする機能を無効にして (`"go.formatOnSave": false`)、入力中にトリガーされないようにします。

_注意 2_: この拡張機能は入力時に `gocode` を使用して補完リストを提供します。
未作成の依存関係を含め、新しい結果を提供するために、拡張機能は `gocode` の `autobuild=true` 設定を使用します。
自動補完に関するパフォーマンスの問題が発生した場合は、VS Codeの設定で `"go.gocodeAutoBuild"：false` を設定してみてください。

### オプション

Goの拡張機能では次のVisual Studio Code設定を使用できます。
これらはユーザー設定 (`cmd+,`) やワークスペース設定 (`.vscode/settings.json`) で設定できます。

```javascript
{
	"go.buildOnSave": true,
	"go.lintOnSave": true,
	"go.vetOnSave": true,
	"go.buildTags": "",
	"go.buildFlags": [],
	"go.lintTool": "golint",
	"go.lintFlags": [],
	"go.vetFlags": [],
	"go.coverOnSave": false,
	"go.useCodeSnippetsOnFunctionSuggest": false,
	"go.formatOnSave": true, 
	"go.formatTool": "goreturns",
	"go.formatFlags": [],
	"go.goroot": "/usr/local/go",
	"go.gopath": "/Users/lukeh/go",
	"go.gocodeAutoBuild": false
}
```

### リンター

リンターはコーディングスタイルのフィードバックと候補を与えるツールです。
デフォルトでは、この拡張機能は公式の [golint](https://github.com/golang/lint) をリンターとして使用します。

デフォルトのリンターを変更し、より高機能な [Go Meta Linter](https://github.com/alecthomas/gometalinter) を使用することができます。
パッケージを手動でインストールする必要があることに注意してください: `go get -u github.com/alecthomas/gometalinter` を実行して、構成を編集してください (`go.lintTool` の値を "gometalinter" に設定する)。

Go meta linterはさまざまなリンターのコレクションを使用しますが、それらのリンターも手動でインストールする必要があります。
いずれかのツールが利用可能な場合、それはデフォルトで使用されます (`golint` はデフォルトではまだ実行されています)。

非常に便利なリンターツールのいくつか:

* [errcheck](https://github.com/kisielk/errcheck) は、コード内のチェックされていないエラーをチェックします。
* [varcheck](https://github.com/opennota/check) は、未使用のグローバル変数と定数を検索します。
* [deadcode](https://github.com/tsenart/deadcode) は、未使用のコードを検出します。

サポートされているすべてのリンターツールをインストールする場合、gometalinterはインストーラを提供します: `gometalinter --install`

特定のリンターのみを実行したい場合（一部のリンターは遅い）、構成を変更してそれらを指定することができます:

```javascript
  "go.lintFlags": ["--disable-all", "--enable=errcheck"],
```

最後に、これらのリンターの結果がコードの右側に表示され（候補が下線付きの場所）、出力ペインにも表示されます。

### コマンド

統合された編集機能に加えて、拡張機能はコマンドパレットにGoファイルを操作するためのいくつかのコマンドを提供します:

* `Go: Add Import` は、Goコンテキストにパッケージの一覧からimport文を追加します
* `Go: Current GOPATH` は、現在設定されているGOPATHを確認します
* `Go: Run test at cursor` は、アクティブなドキュメントの現在のカーソル位置でテストを実行します
* `Go: Run tests in current package` は、アクティブなドキュメントを含むパッケージ内のすべてのテストを実行します
* `Go: Generates unit tests (package)` は、現在のパッケージのユニットテストを生成します
* `Go: Generates unit tests (file)` は、現在のファイルのユニットテストを生成します
* `Go: Generates unit tests (function)` は、現在のファイルで選択された関数のユニットテストを生成します

### _任意_: デバッグ

デバッガーを使用するには、現在 `delve` を手動でインストールする必要があります。
詳細については [インストール手順](https://github.com/derekparker/delve/tree/master/Documentation/installation) を参照してください。
OS Xでは、`dlv` バイナリーに署名するために自己署名証明書を作成する必要があります。

詳細については [VS Codeを使用したGoコードのデバッグ](https://github.com/Microsoft/vscode-go/wiki/Debugging-Go-code-using-VS-Code) を参照してください。

#### リモートデバッグ

VS Codeを使用したリモートデバッグを行うには [リモートデバッグ](https://github.com/Microsoft/vscode-go/wiki/Debugging-Go-code-using-VS-Code#remote-debugging) を参照してください。

## 拡張機能の構築とデバッグ

あなたは拡張機能開発中に拡張機能をデバッグするための開発環境をセットアップできます。

はじめに、`~/.vscode/extensions` に拡張機能がインストールされていないことを確認してください。
つぎに、マシン上のどこか別の場所にリポジトリーをクローンし、`npm install` を実行してCodeの開発インスタンスを開きます。

```bash
rm -rf ~/.vscode/extensions/lukehoban.Go
cd ~
git clone https://github.com/Microsoft/vscode-go
cd vscode-go
npm install
code .
```

デバッグビューレットに移動し、`Launch Extension` を選択してrun (`F5`) を押します。

`[Extension Development Host]` インスタンスで、GOPATHフォルダーを開きます。

ブレークポイントを設定して、拡張機能をステップ実行することができます。

拡張子 `.ts` ファイルを編集した場合は、Codeの `[Extension Development Host]` インスタンスをリロード (`cmd-r`) して新しい拡張機能コードをロードしてください。
デバッグインスタンスが自動的に再接続されます。

デバッガーをデバッグするには、[the debugAdapter readme](src/debugAdapter/Readme.md) を参照してください。

## ツール

この拡張機能は、現在のGOPATHにインストールされている次のツールを使用します。
いずれかのツールが見つからない場合は、エディターの右下隅に "Analysis Tools Missing" という警告が表示されます。
その警告をクリックすると、不足しているツールをインストールすることができます。

- gocode: `go get -u -v github.com/nsf/gocode`
- godef: `go get -u -v github.com/rogpeppe/godef`
- gogetdoc: `go get -u -v github.com/zmb3/gogetdoc`
- golint: `go get -u -v github.com/golang/lint/golint`
- go-outline: `go get -u -v github.com/lukehoban/go-outline`
- goreturns: `go get -u -v sourcegraph.com/sqs/goreturns`
- gorename: `go get -u -v golang.org/x/tools/cmd/gorename`
- gopkgs: `go get -u -v github.com/tpng/gopkgs`
- go-symbols: `go get -u -v github.com/newhook/go-symbols`
- guru: `go get -u -v golang.org/x/tools/cmd/guru`
- gotests: `go get -u -v github.com/cweill/gotests/...`

拡張機能にツール用の別のGOPATHを使用させる場合は、VSCODE_GOTOOLS 環境変数を目的のパスに設定します。

現在のGOPATHに手動でツールをインストールするには、以下をペーストして実行します:

```bash
go get -u -v github.com/nsf/gocode
go get -u -v github.com/rogpeppe/godef
go get -u -v github.com/zmb3/gogetdoc
go get -u -v github.com/golang/lint/golint
go get -u -v github.com/lukehoban/go-outline
go get -u -v sourcegraph.com/sqs/goreturns
go get -u -v golang.org/x/tools/cmd/gorename
go get -u -v github.com/tpng/gopkgs
go get -u -v github.com/newhook/go-symbols
go get -u -v golang.org/x/tools/cmd/guru
go get -u -v github.com/cweill/gotests/...
```

そして、デバッグのためには:

- delve: https://github.com/derekparker/delve/blob/master/Documentation/installation/README.md の指示に従ってください。

## ライセンス
[MIT](LICENSE)
