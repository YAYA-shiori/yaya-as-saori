# YAYA as SAORI

## 概要

yaya.dllをSAORIとして呼び出すモジュールです。

## ファイル説明

### 「yaya_saori」フォルダ

- `yaya.dll`: umeichiさんが開発されたaya.dllを整備班さんが引き継いで開発されているdllです。YAYA 6（600系）です。
- `builtins.aym`: 摂理さんが作成されたYAYAビルトイン（システム）関数設定ファイルです。postic管理人さん、他複数の方が改変しています。YAYA本体のシステム関数を呼び出す仕組みと、戻り値の返し方が特殊な関数だけを書いてあります。個々のシステム関数のwrapperは書いていません（「保守のしかた」を参照）。
- `ayasaori.aym`: 摂理さんが作成されたyaya.dll設定ファイルです。nikolatさんが[こちら](https://gist.github.com/nikolat/1007538)で公開されているコードに入れ替えてあります。リクエストヘッダの解析には `PARSEHEADER` を使っています。

### 「readme_old」フォルダ

- `readme.txt`、`readme_080104.txt`: 摂理さんが作成されたreadmeです。
- `sample.aym`: 摂理さんが作成されたモジュールサンプルです。
- `dic_sample.txt`: 摂理さんが作成された `sample.aym` モジュールをテストするための里々辞書です。

## 使いかた

次の4つのファイルを同じ階層に置きます。

- `yaya.dll`
- `yaya.txt`
- `ayasaori.aym`
- `builtins.aym`

SHIORIからこの `yaya.dll` をパス指定し、SAORIとして呼び出します。

| 引数 | 内容 |
|---|---|
| `Argument0` | 使用したいYAYAの関数名 |
| `Argument1` | 使用したいYAYAの関数の引数1 |
| `Argument2` | 使用したいYAYAの関数の引数2 |
| `Argument3` | 使用したいYAYAの関数の引数3 |
| `Argument4...` | 以下同じ |

YAYAの関数については[文Wikiの関数一覧](http://emily.shillest.net/ayaya/index.php?FrontPage)を参照してください。仕様上、SAORIとして利用できない関数もあります。

YAYA本体のシステム関数はすべてそのまま呼び出せます。YAYAに関数が増えても自動で使えます。関数名の大文字小文字は問いません（`STRLEN` でも `strlen` でも可）。

- 引数: すべて文字列で届きます。数値が必要な引数は自動で変換します。
- 整数・実数・文字列の戻り値: `Result` に入ります。
- 配列の戻り値: `Value0`、`Value1`、... に入ります。一部の関数は `Result` に個数なども入ります。
- ハッシュの戻り値: `Result` に `SERIALIZEVAR` と同じ書式の文字列で入ります。
- 引数が足りないとき: `400 Bad Request` を返します。

里々での使い方の説明と里々用のデモゴーストは、[postic](http://navy.nm.land.to/post/)にあります。

## 保守のしかた

YAYAのシステム関数を呼び出すだけなら、何も書く必要はありません。次のときだけ `builtins.aym` に書き足します。

- 数値で渡したい引数がある関数
  - `AYASAORI.ArgSig` に型を書きます。一文字が一つの引数です。
  - 書かなかった引数や関数は、文字列のまま渡します。
- 配列を返す関数で、`Result` にも個数や連結文字列を入れたい関数
  - `AYASAORI.ArrayResult` に書きます。`count` は個数、`join` は `,` でつないだ文字列です。
  - 書かなかった関数は `Value0..` だけを返します。
- それ以外の特殊な返し方をしたい関数
  - 関数名を小文字にした関数（例: `fread`）を書きます。そちらが優先されます。
  - 引数は `REQ.ARGS[0]`、`REQ.ARGS[1]`、...、結果は `RES.RESULT` と `RES.VALUE0`、`RES.VALUE1`、... に入れます。
  - 引数が足りないときは `-1` を返すと `400 Bad Request` になります。
- 自分のモジュール（`sample.aym` のようなもの）
  - これまでどおり、`yaya.txt` に `dic` として足すだけです。

`AYASAORI.ArgSig` の型の記号は次のとおりです。

| 記号 | 渡し方 |
|---|---|
| `s` | 文字列 |
| `i` | 整数 |
| `r` | 実数 |
| `a` | 整数・実数・文字列を見分ける |
| `x` | `a` と同じ。ただし `"007"` は文字列のまま |

大文字は省略できる引数です。末尾にあって空のときは渡しません。

### 書くときの注意

- YAYAの `#define` は書いたファイルの中でしか使えません。`ayasaori.aym` で `#define` したものを `builtins.aym` で使うことはできません。
- `{ }` の中で初めて代入したローカル変数は、その `{ }` の中でしか使えません。

## 旧版（Tc571系）からの変更点

- ヘッダの解析を `PARSEHEADER` に変えました。LF区切りのリクエストも受け付けます。
- 前のリクエストの `REQ.ARGUMENTn` が次のリクエストに混ざることがあった問題を直しました。
- `builtins.aym` にwrapperが無かった関数（`SQL*`、`PARSE*`、`FREAD*` などのファイル形式の関数、`HASH_*` など）が、`200 OK` で空の応答を返していた問題を直し、呼び出せるようにしました。
- `lint.*` が呼び出しの深さの限界で止まっていた問題を直しました。`LINT.GetVarRefs` も呼べます。
- `GETFUNCLIST`、`GETSYSTEMFUNCLIST` の絞り込みが効いていなかった問題を直しました。
- `GETTIME` の第2引数（タイムゾーン）、`FENUM` の第2引数、`SPLIT`、`RE_SPLIT` の個数、`CHARSETIDTOTEXT`、`FWRITEBIN` が正しく渡るようにしました。
- `RE_SPLIT` の `Value0..` は区切りにマッチした文字列ではなく、分割された文字列になりました。

## ライセンス

Unlicense licenseとします。
歴代のメンテナの方々に了承を得るべきところでしょうけれども
これまでの彼らのスタンスから鑑みて快諾してくれるものと信じます。

## 更新履歴

コミットログ: <https://github.com/YAYA-shiori/yaya-as-saori/commits/main>

- 2026.11.02 (v2.0.0)
  - yaya.dllをTc607-3（YAYA 6）に変更
  - YAYA本体のシステム関数を、個別のwrapperなしで呼び出せるように変更
  - リクエストヘッダの解析をPARSEHEADERに変更
  - 詳しくは「旧版（Tc571系）からの変更点」を参照
- 2021.11.12
  - builtins.aymにsleepを追加
  - yaya.dllを Tc564-1に変更
  - ファイルの文字コードをShift_JISからUTF-8に変更
  - ※配布場所をGitHubに移行 以降の更新履歴はコミットログを参照
  - 編集者: Don
- 2021.10.08
  - builtins.aymにisevaluableとsettamahwndとisglobaldefineとsetglobaldefineとappend_runtime_dicを追加
  - yaya.dllを Tc562-1に変更
  - 編集者: Don
- 2021.08.31
  - builtins.aymにundeffuncとundefglobaldefineとdicunloadを追加
  - messagetxtフォルダ内の言語ファイルを差し替え
  - yaya.dllを Tc561-2に変更
  - 編集者: Don
- 2021.07.16
  - builtins.aymにprocessglobaldefineを追加
  - yaya.dllを Tc559-1に変更
  - 編集者: Don
- 2021.07.11
  - builtins.aymにgetfuncinfoを追加
  - messagetxtフォルダ内の言語ファイルを差し替え
  - yaya.dllを Tc558-2に変更
  - 編集者: Don
- 2021.07.10
  - builtins.aymにgetsystemfunclistを追加
  - ログに記録されるエラーメッセージ類の言語ファイルを同梱
  - yaya.dllを Tc557-1に変更
  - 編集者: Don
- 2021.06.30
  - builtins.aymのgetsettingで引数の数値指定が一部動作していなかった問題を修正
  - yaya.dllを Tc556-2に変更
  - 編集者: Don
- 2021.06.27
  - builtins.aymにdicloadを追加
  - yaya.dllを Tc556-1に変更
  - 編集者: Don
- 2021.06.25
  - builtins.aymのfwritedecodeで第3引数(type)を省略できるよう変更
  - builtins.aymにgeterrorlogを追加(戻り値が配列なのでSPLITとかと同様Value*に格納しました)
  - yaya.dllを Tc555-2に変更
  - 編集者: Don
- 2021.06.24
  - builtins.aymにfwritedecodeを追加
  - yaya.dllを Tc554-1に変更
  - 編集者: Don
- 2021.06.17
  - builtins.aymのfreadencodeで第2引数(size)が整数でなく文字列として認識され
  - 常にエラーが返されていた問題を修正
  - 編集者: Don
- 2021.06.14
  - builtins.aymのstrencodeで第3引数が指定できなかった問題を修正
  - builtins.aymのstrdecodeで第3引数が指定できなかった問題を修正
  - builtins.aymにsrandとgetenvを追加
  - yaya.dllを Tc553-1に変更
  - 編集者: Don
- 2015.12.30
  - builtins.aymのrandが動作していなかった問題を修正
  - yaya.dllを Tc549-1に変更
  - 編集者: もっしょくし
- 2014.11.08
  - builtins.aymのgetseccountの記述修正
  - ・引数無指定時にエラーになっていた問題
  - ・各引数について空を指定した場合に0を指定した事になってしまう問題
  - 編集者: もっしょくし
- 2014.11.08
  - yaya.dllを Tc548-2に変更
  - builtins.aymのreplace、re_replace、re_replaceexの仕様追加に対応し、置換回数オプションを追加
  - 編集者: もっしょくし
- 2014.01.03
  - 文wikiにて公開。
