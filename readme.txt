
YAYA as SAORI


■概要
yaya.dllをSAORIとして呼び出すモジュールです。


■ファイル説明

「yaya_saori」フォルダ

・yaya.dll
umeichiさんが開発されたaya.dllを整備班さんが引き継いで開発されているdllです。

・builtins.aym
摂理さんが作成されたYAYAビルトイン（システム）関数設定ファイルです。
postic管理人さん、他複数の方が改変しています。

・ayasaori.aym
摂理さんが作成されたyaya.dll設定ファイルです。
nikolatさんがこちらで公開されているコードに入れ替えてあります。
https://gist.github.com/nikolat/1007538


「readme_old」フォルダ

・readme.txt
・readme_080104.txt
摂理さんが作成されたreadmeです。

・sample.aym
摂理さんが作成されたモジュールサンプルです。

・dic_sample.txt
摂理さんが作成された"sample.aym" モジュールをテストするための里々辞書です。


■使いかた
yaya.dll
yaya.txt
ayasaori.aym
builtins.aym

以上の4つを同じ階層に置く。
SHIORIからこの「yaya.dll」をパス指定しSAORIとして呼び出す。

　Argument0 使用したいYAYAの関数名
　Argument1 使用したいYAYAの関数の引数1
　Argument2 使用したいYAYAの関数の引数2
　Argument3 使用したいYAYAの関数の引数3
　Argument4...（以下同じ）

YAYAの関数については文Wikiの関数一覧参照。
http://emily.shillest.net/ayaya/index.php?FrontPage
仕様上、SAORIとして利用できない関数もあります。


里々での使い方説明、里々用デモゴーストはこちら。
postic　http://navy.nm.land.to/post/


■ライセンス
Unlicense licenseとします。
歴代のメンテナの方々に了承を得るべきところでしょうけれども
これまでの彼らのスタンスから鑑みて快諾してくれるものと信じます。


■更新履歴
コミットログ
https://github.com/YAYA-shiori/yaya-as-saori/commits/main

2021.11.12　builtins.aymにsleepを追加
　　　　　　yaya.dllを Tc564-1に変更
　　　　　　ファイルの文字コードをShift_JISからUTF-8に変更
　　　　　　※配布場所をGitHubに移行 以降の更新履歴はコミットログを参照
　　　　　　[編集者：Don]
2021.10.08　builtins.aymにisevaluableとsettamahwndとisglobaldefineとsetglobaldefineとappend_runtime_dicを追加
　　　　　　yaya.dllを Tc562-1に変更
　　　　　　[編集者：Don]
2021.08.31　builtins.aymにundeffuncとundefglobaldefineとdicunloadを追加
　　　　　　messagetxtフォルダ内の言語ファイルを差し替え
　　　　　　yaya.dllを Tc561-2に変更
　　　　　　[編集者：Don]
2021.07.16　builtins.aymにprocessglobaldefineを追加
　　　　　　yaya.dllを Tc559-1に変更
　　　　　　[編集者：Don]
2021.07.11　builtins.aymにgetfuncinfoを追加
　　　　　　messagetxtフォルダ内の言語ファイルを差し替え
　　　　　　yaya.dllを Tc558-2に変更
　　　　　　[編集者：Don]
2021.07.10　builtins.aymにgetsystemfunclistを追加
　　　　　　ログに記録されるエラーメッセージ類の言語ファイルを同梱
　　　　　　yaya.dllを Tc557-1に変更
　　　　　　[編集者：Don]
2021.06.30　builtins.aymのgetsettingで引数の数値指定が一部動作していなかった問題を修正
　　　　　　yaya.dllを Tc556-2に変更
　　　　　　[編集者：Don]
2021.06.27　builtins.aymにdicloadを追加
　　　　　　yaya.dllを Tc556-1に変更
　　　　　　[編集者：Don]
2021.06.25　builtins.aymのfwritedecodeで第3引数(type)を省略できるよう変更
　　　　　　builtins.aymにgeterrorlogを追加(戻り値が配列なのでSPLITとかと同様Value*に格納しました)
　　　　　　yaya.dllを Tc555-2に変更
　　　　　　[編集者：Don]
2021.06.24　builtins.aymにfwritedecodeを追加
　　　　　　yaya.dllを Tc554-1に変更
　　　　　　[編集者：Don]
2021.06.17　builtins.aymのfreadencodeで第2引数(size)が整数でなく文字列として認識され
　　　　　　常にエラーが返されていた問題を修正
　　　　　　[編集者：Don]
2021.06.14　builtins.aymのstrencodeで第3引数が指定できなかった問題を修正
　　　　　　builtins.aymのstrdecodeで第3引数が指定できなかった問題を修正
　　　　　　builtins.aymにsrandとgetenvを追加
　　　　　　yaya.dllを Tc553-1に変更
　　　　　　[編集者：Don]
2015.12.30　builtins.aymのrandが動作していなかった問題を修正
　　　　　　yaya.dllを Tc549-1に変更
　　　　　　[編集者：もっしょくし]
2014.11.08　builtins.aymのgetseccountの記述修正
　　　　　　・引数無指定時にエラーになっていた問題
　　　　　　・各引数について空を指定した場合に0を指定した事になってしまう問題
　　　　　　[編集者：もっしょくし]
2014.11.08　yaya.dllを Tc548-2に変更
　　　　　　builtins.aymのreplace、re_replace、re_replaceexの仕様追加に対応し、置換回数オプションを追加
　　　　　　[編集者：もっしょくし]
2014.01.03　文wikiにて公開。
