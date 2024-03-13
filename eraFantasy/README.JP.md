# EmueraEE

Document: [日本語(Japanese)](https://evilmask.gitlab.io/emuera.em.doc/) / [简体中文(Simplified Chinese)](https://evilmask.gitlab.io/emuera.em.doc/zh/)

- タイトル：EmueraEE 最終更新日:2022/05/21
- バージョン：1.824+v15+EMv7+EEv14
- 改変者：Enter
- 元となったアプリケーション：Emuera1.824+v15（妊）|д ﾟ)の中の人、及び MinorShift 制作）、WebP-wrapper(JosePineiro 制作)、Emuera.EM（EvilMask 制作）
- 連絡先：[Twitter](https://twitter.com/erabemani) / [Discord](https://discord.gg/p5rb5uK)

※eramaker の作者様及び MinorShift 様、妊の人様は EmueraEE の製作には関与していません。  
　当アプリの不具合は上記連絡先にご報告ください

※同梱の Emuera-Anchor-EE は英語圏の era コミュニティで使用されている「Emuera-Anchor」を EE に対応させたものです  
　各 UI やエラーメッセージ等が英語になっています。必要に応じて使い分けてください

※使用しているセキュリティソフト次第では危険なファイルとして警告・削除される場合があります  
　セキュリティソフトの設定を変更して使用することはできますが、自己責任でお願いします  
　 virustotal(ファイルの安全性確認サイト)のリンク：[VirusTotal](https://www.virustotal.com/gui/file/9313aad680fa35e95fce95ad0477f4d2656df07310af18e7a61f25924f7a19ec)

> v12 にて Emuera.EM と機能統合。詳しくは同梱の Emuera.EM_read me.txt をご覧ください

## 概要

Emuera をベースに改造を施したバージョンです。追加された機能は以下の通り

### PLAYSOUND "ファイル名.拡張子"

「sound」フォルダ内にある音声ファイルを 1 回再生します  
v12 で 10 チャンネルに対応。最大 10 個のファイルを同時再生できる

### STOPSOUND

PLAYSOUND で再生中の音声ファイルを停止します

### PLAYBGM "ファイル名.拡張子"

「sound」フォルダ内にある音声ファイルをループ再生します

### STOPBGM

PLAYBGM で再生中の音声ファイルを停止します

### EXISTSOUND "ファイル名.拡張子"

「sound」フォルダ内に指定した音声ファイルが存在するか判定します。存在すれば 1 が、しなければ 0 が RESULT に入ります

### EXISTSOUND("ファイル名.拡張子")

同名命令の式中関数版

### SETSOUNDVOLUME int

PLAYSOUND の音量を変更します。引数には 0 ～ 100 を指定できます

### SETBGMVOLUME

PLAYBGM の音量を変更します。引数には 0 ～ 100 を指定できます

Windows Media Player のライブラリを使っているので WMP がインストールされてる PC であれば、  
WMP で動く音声ファイルは全て動くと思います。

### INPUTMOUSEKEY でボタン使用可能に

命令実行時に RESULT:0=1(マウスクリック時)だった場合に RESULT:5 にボタンの数値が入ります

### GDRAWTEXT gID, "テキスト", X 座標, Y 座標

指定した gID にテキストを描写する 座標は省略可能  
GSETBRUSH cARGB と組み合わせることで文字の濃度と色を変更できる cARGB は濃度+文字色の 16 進数 8 桁  
成功すれば RESULT:0 に 1 が、RESULT:1 には生成したテキスト画像の幅が、RESULT:2 には高さが入る(高さはほぼ GSETFONT で指定した値)  
v7plus から描画フォーマットを GenericTypographics に変更 実際の PRINT 描画と同じ位置になるように

### GGETFONT gID

指定した ID の、GSETFONT で指定したフォント名を呼び出す

### GGETFONTSIZE gID

指定した ID の、GSETFONT で指定したフォントサイズを呼び出す

### OUTPUTLOG でファイル名と拡張子を指定可能に

OUTPUTLOG に引数指定することでそのファイル名.拡張子で出力できる リテラルは PRINTS とかと同じ  
v5fix で親ディレクトリを指定できる脆弱性を修正 子ディレクトリは指定可能

### EXISTFUNCTION("関数名")

名前の通り関数が存在するかの式中関数 通常関数なら 1 を、式中関数(数値型)なら 2 を、式中関数(文字列型)なら 3 を返す  
TRYC 系で CALL 機能を付随させたくない場合にお使いください また、システム関数やシステム組み込み式中関数は 0 を返す

### GSETFONT でフォントスタイルを指定できるように

第 4 引数に SETFONT と同じ 4 ビット数(1=太字 2=斜体 4=打ち消し線 8=下線)指定で装飾を付けられるように 省略可能

### GGETFONT gID

指定した ID の、GSETFONT で指定したフォントスタイルを数値で返す

### GGETTEXTSIZE "テキスト", フォント名, フォントサイズ, フォントスタイル

指定の引数で GDRAWTEXT を行った場合のサイズを取得する 第 4 引数(フォントスタイル)は省略可能  
FONTBOLD 相当のスタイルの場合、サイズが微妙に大きくなるため注意

### TRYCALLF 関数名

CALLF の TRY 命令 CALLF 同様に返り値は破棄されるが、関数が無くてもエラーにならない

### TRYCALLFORMF 関数名

CALLFORMF の TRY 命令 CALLFORMF 同様に返り値は破棄されるが、関数が無くてもエラーにならない  
上記 2 つを TRYC ～ CATCH 式に使う場合は EXISTFUNCTION と併用してください

### GDRAWGWITHROTATE コピー先 gID, コピー元 gID, 回転させる角度(時計回り), X 座標, Y 座標

コピー元 ID のイメージを、指定した角度回転させてコピー先 ID に貼り付ける 座標は省略可能で省略すると中心点(X/2,Y/2)になる  
角度はマイナスを指定すれば反時計回りになる 360 を超過しても問題ない  
中心点はコピー元のサイズに準拠しているが、サイズの違う gID を扱うとズレが生じる 角度に応じてズレの度合いが違う(90°、270° で最大)ので、その際は中心点を変えてあげるとよい

左のように 200*100 を 90° 回転させて 100*200 の g に移す場合、太枠の 100,50 地点を中心に回転するため右のようにずれる  
なので重なってる部分の中心点、つまり 50,50 地点(・)を中心点に指定すればきれいにハマる 270° 回転なら全体の中心点、100,100 を指定するとハマる

```
┏━┯━┓ ―┏━┓
┃・│―┃ ┌╂┐┃
┣━┿━┛ │┃│┃
│―│―― │┗┿┛
└─┘―― └─┘―
```

### QUIT_AND_RESTART

再起動命令。QUIT 同様に WAIT1 回相当の入力待ちがある

### FORCE_QUIT

入力待ちせずに QUIT する命令

### FORCE_QUIT_AND_RESTART

入力待ちせずに QUIT_AND_RESTART する命令 入力待ちを挟まず連続実行されるとダイアログボックスで警告が出る

### FORCE_BEGIN システム関数

BEGIN の制約を受けずに強制的に BEGIN を実行する。フローを意図的に壊すため、予期せぬ不具合が起こる可能性があります

### WebP に対応

がめら氏作の「src1824+v11+webp+Secure」を参考に WebP 読み込み機能を追加  
あくまで参考で(=完全マージではない ベースバージョンが違うため)、ファイル読み込み処理だけ WebP 対応にしただけなので元の WebP 版には無いエラーが起こる可能性があります  
WebPWrapper については東 eto マン氏、M 氏が改良を行ったものを使用しています 僭越ながら同梱の「WebP 版 read me」に文書をまとめさせていただきました 感謝  
EMv6+EEv13 にてセキュリティ上の観点から別のライブラリに変更しました。同梱の libwebp.dll を Emuera と同ディレクトリにコピーしてお使いください

### ERH で定義した変数に csv ファイルで名前を付けられるように

ERH で定義した変数名を準拠にファイルを読み込み、既存の csv 変数と同じように配列に名前を付けることができる。現時点では一次元配列変数にのみ対応  
CSV フォルダ内で使えるものは従来どおり「変数名.csv」、ERB 内で使えるものは「変数名.ERD」ファイルとなる。書式は CSV 変数のファイルと同じ。これらが 2 つ以上存在する場合は起動時にエラー吐いて終了する  
ただし ABLNAME や TRAINNAME に相当する変数は無し。今後命令や式中関数を実装する予定

### GETMEMORYUSAGE()
現在起動中のEmueraのメモリ使用量を返す（byte）。ワーキングセットの値なのでタスクマネージャーの数値とは差異が生じます

### CLEARMEMORY()
メモリを解放する。解放されたメモリ量（byte）を返す  
DELCHARAやセーブデータのロード、RESETDATA等データ削除した際に使うと効果がありますが、それらが行われないタイミングで使用しても特に効果はありません  
そこそこ重いため乱用は控えたほうが無難

### UPDATECHECK

アップデートチェック命令を追加。以下使い方

1. GameBase.csv に「バージョン名」「バージョン情報 URL」を追加  
   　バージョン名には文字列型で任意のバージョン名、URL にはバージョン確認用の URL を記載する
2. 上記の URL にバージョン情報と最新版リンクを記載したテキストをアップロードする  
   　 1 行目に最新のバージョン名を書き、2 行目にはリンクを書く 3 行目以降はコメント等自由に書いて OK  
   　オープンな Git を使うならリポジトリにプッシュして raw で参照するのもあり
3. UPDATECHECK 実行時に GameBase.csv に記載した URL にアクセスし、現バージョンと最新バージョンが違えばダイアログボックス経由でサーバー側のリンクを開くかどうかプレイヤーに尋ねる  
   　最新版の場合は RESULT に 0 が入って終了する
4. 「はい」が選ばれればブラウザを開き RESULT に 2 が入る 「いいえ」が選ばれれば何もせず RESULT に 1 が入る リンク先が見つからない、リンク先にバージョン情報や最新版リンクが書いてない等、何らかの理由で失敗した場合は RESULT には 3 が入る  
   　また、コンフィグに「UPDATECHECK を許可しない」の項目を追加、これがオンになってるときに UPDATECHECK を実行した場合は何もしないが、RESULT に 4 が入る  
   11fix にてネットワークに接続されていない場合は RESULT に 5 が入るように

---

※※※※※※※※※※※※※※※※※※※※※※  
製作者の方々へ 上記の機能を利用して悪意あるリンク等を記載しユーザーを騙す行為は不正アクセス禁止法に抵触し、厳重な処罰を受ける場合があります 良識を持った使い方を心がけましょう  
ユーザーの方々へ リンクを開く際はそれが安全で信頼できるアドレスかを確認し、慎重に開くようにしましょう  
※※※※※※※※※※※※※※※※※※※※※※

---

## 使用方法

Emuera が正常に動作する era バリアントのディレクトリにて Emuera と同様の方法でお使いください

## ライセンス

同フォルダ内の Emuera_readme.txt の[ライセンス]の項に準じます  
私、Enter は一切の権利を主張しません  
また、当 Emuera を使用、同梱及び配布したバリアントについて生じた問題には責任を負いかねます

## Other readme files

### 本家 Emuera

```
タイトル：Emuera
バージョン： 1.824
著作者：MinorShift, 妊）|дﾟ)の中の人
頒布者：MinorShift
連絡先：minorshift@hotmail.co.jp
連絡先：https://twitter.com/MinorShift
Webページ：http://osdn.jp/projects/emuera/
投げ銭・寄付：https://emuera.booth.pm/items/933704
ライセンス：[ライセンス]の項を参照。
更新の詳細は上記Webページをご覧下さい。

[Emueraについて]
Emuera（エミューラ）はeramakerのエミュレータ＋αです。
eramaker.exeと同じフォルダにいれて起動することでeramaker.exeの代わりにERBスクリプトを実行します。
マウスで操作したり簡単なマクロ機能を使用することなどができます。


[動作環境]
.NET Framework 4.5環境が必要です。

「アプリケーションを正しく初期化できませんでした」というエラーが出て起動できない場合、.NET Framework 4.5が正しくインストールされていません。
Microsoftダウンロードセンターから.NET Framework 4.5ランタイムをダウンロードしてインストールしてください。


[使用方法]
EmueraXXXX.exeをeramaker.exeと同じフォルダに入れて起動してください。
(XXXXにはバージョンを表す数字が入ります。また、環境によっては.exeが表示されないことがあります)
起動しない場合、[動作環境]の節をお読み下さい。
起動直後にemuera.config、エラー終了した場合にはemuera.logを生成します。
詳細は
http://osdn.jp/projects/emuera/wiki/FrontPage
を参照してください。

[その他の頒布物]
ソースコードや拡張機能のサンプルなどを以下のページで頒布しています。
http://osdn.jp/projects/emuera/releases/
のソースコードまたは関連ファイルからダウンロードしてください。

[eramakerについて]
eramakerはサークル獏の佐藤敏氏が作成したソフトです。
詳細は下記サイトを参照してください。
http://www.hakagi.com
※18歳未満の方は閲覧をご遠慮ください。

[投げ銭・寄付]
https://emuera.booth.pm/items/933704
Emueraの作者の一人MinorShiftは上記サイトにて投げ銭を募っております。
ご支援頂ければ幸いです。

[連絡先]
minorshift@hotmail.co.jp

※eramakerの作者様はEmueraの製作には関与していません。
Emueraへの苦情やバグ報告などをサークル獏や佐藤氏宛てに送らないで下さい。


[ライセンス]（zlib/libpngライセンス）
Copyright (C) 2008- MinorShift, 妊）|дﾟ)の中の人

本ソフトウェアは「現状のまま」で、明示であるか暗黙であるかを問わず、何らの保証もなく提供されます。 本ソフトウェアの使用によって生じるいかなる損害についても、作者は一切の責任を負わないものとします。

以下の制限に従う限り、商用アプリケーションを含めて、本ソフトウェアを任意の目的に使用し、自由に改変して再頒布することをすべての人に許可します。

1.本ソフトウェアの出自について虚偽の表示をしてはなりません。あなたがオリジナルのソフトウェアを作成したと主張してはなりません。 あなたが本ソフトウェアを製品内で使用する場合、製品の文書に謝辞を入れていただければ幸いですが、必須ではありません。
2.ソースを変更した場合は、そのことを明示しなければなりません。オリジナルのソフトウェアであるという虚偽の表示をしてはなりません。
3.ソースの頒布物から、この表示を削除したり、表示の内容を変更したりしてはなりません。
```

### 私家改造版 Emuera

```
◎私家改造版について
本家Emueraのバグ修正を行ったものとなります。

◎開発方針
Emuera本家が対応停止状態となっているため、私家改造版はコードのベースとするためにバグ修正以外の変更を行わないものとします

◎ライセンス
Emuera本体に準じます。

◎派生ソフトの作成について
Emueraのライセンスに従っていただければ自由にフォークしていただいて構いません。
むしろ、新機能が欲しい等の場合には積極的にフォークしていただければと思います。
その際はEmueraのライセンスファイルの添付とともに、フォーク元になったベースバージョンの明記もお願いいたします。
（Emueraのライセンスファイルが当ソフトのライセンスファイルを兼ねるため、本ファイルの添付は不要です。）

◎現在のベースバージョン
・Emuera1824+v15
　○LOADTEXTでEmueraでは絶許である\rが紛れ込むあまりにも致命的な処理上の穴を塞いだ
　　　
・Emuera1824+v14
　○resourceフォルダの読み込みで.csvXのようなゴミのついた拡張子もcsvファイルとして読みこんでしまうのを修正
　○CSVでエラーをかました時の行表示処理で例外をはくのを修正
　　　CSVファイルがエラーになった時のこと忘れてた（ﾃﾍﾍﾟﾛ
　　　
・Emuera1824+v13
　○[SKIPSTART]～[SKIPEND]のブロック内に{や}があると、行連結と誤爆して例外を投げるのを修正
　
・Emuera1824+v12
　○ADDSPCHARがSPキャラ互換設定有効時の処理がおかしいのを修正
　○コード自体は内部のに保存せず、エラー時はファイルから読み取るように修正
　　　大規模バリアントだと数百MBぐらいメモリ使ってくれちゃってたので
　○一部コードの微修正
　　　無駄な代入処理な引数を削除してみたり
　○他にも何かあるかも（更新自体が久々過ぎて何やったかもう覚えてない
　　　
・Emuera1824+v11
　○v10で取り込んだ変更を色々チューニング+微修正
　　　古い実装ではUIのレスポンスが悪い+表示位置正しくならないため

◎過去の修正内容
・Emuera1824+v10
　○Emuera1823+v1のツールチップ周りの変更が1824に取り込まれてなかったので追加
　○VARSETが特定条件で正常に機能しないのを修正

・Emuera1824+v9
　○ARRAYMSORTがInt32の範囲しか扱えないのをInt64の範囲全体に拡張
　　　そのままではうまくいかないことも、ちょっと頭をひねればできてしまうものである。
　○内部の変数定義でTYPOがあったのを修正
　　　これまで問題にならなかった＝誰も使ってない　というわかりやすいロジック

・Emuera1824+v8.1
　○例外発生時のエラーログ出力時のファイル名表示がフルパスなのをそうでないように

・Emuera1824+v8
　○再起動時の画像データのクリアがちゃんとできていないのを修正
　○WINAPIモード時の再起動中に内部で例外が起こりそのまま落ちてしまうことがあったのを修正

・Emuera1824+v7
　○ERHファイル中で起きたエラーを正しくキャッチして処理を止められていないバグを修正
　　　そりゃ大量のエラー祭にもなるわけである
　　　ちなみに構文そのものエラーと、行解析時のエラーの両方があるときだけ正しく動いていたとかいうギャグ

・Emuera1824+v6.2
　○UNICODE関数の警告表示周りで事前解析時にRestructureされてる場合に.Net例外になるのを修正
　　　実行中に発覚した場合とは処理変えないといけなかったですね、はい

・Emuera1824+v6.1
　○UNICODE関数周りの変更
　　　改行に対応する制御文字0x0Aと0x0Dは通すように変更
　　　ついでにごりおしで注意文を出すシステムを作ったので、警告かつ空文字列を返すように変更

・Emuera1824+v6
　○UNICODEメソッドが制御文字に対応する数字を受け付けないように
　　　0では実害あるケース報告されてるものの他はあんま影響ないとは思うわけだが念のため
　　　わざとか事故以外ないと思うのでエラー扱いでいいや
　○内部コードの無駄を整理やら保守面での変更やら
　　　雑なコードの多さが露わすぎたので

・Emuera1824+v5
　○STRJOIN内部処理の引数ハンドリングがザルで事故りまくりなのを修正
　　　ほんと実装が中途半端すぎたorz

・Emuera1824+v4
　○+v1の修正が中途半端でやっぱりエラー吐くのを修正
　　　チェックが甘かった
　○HTML_PRINTの<button>タグでvalueが32bit整数を超える値の場合ボタンにならないのを修正
　　　実用的には問題にならない気もするが

・Emuera1824+v3
　○行連結読み込み時にFORM文字列{数値変数}が先頭にあると豪快に誤爆して例外を放り投げてくるのを修正
　　　そりゃ、この判定の仕方じゃ誤爆するよ、TrimStartしかしてないんだよ？

・Emuera1824+v2
　○HTML_GETPRINTEDSTRが正しくない返り値を返すケースがあるのを修正
　　　こ　れ　は　ひ　ど　い

・Emuera1824+v1（差し替え版）
　○STRJOINが豪快にバグってたどころか、仕様に対して実装がgdgdになっていたのを修正
　　　おそらく、引数の仕様変えた時に実装が中途半端だったっぽいorz

・Emuera1823+v1
　○TOOLTIP_SETDELAYとTOOLTIP_SETDURATIONを併用した場合にDELAYが働かないバグを修正
　　　C#側に併用できる仕組みがないのが悪いんや…
　○テスト版で行った残り時間非表示のTINPUTとAWAITの挙動変更を取り込み
　　　もちろんがんがんテスト状態、事故ったらﾜｰｲヽ(ﾟ∀ﾟ)ﾒ(ﾟ∀ﾟ)ﾒ(ﾟ∀ﾟ)ﾉﾜｰｲ

・Emuera1822+v1
　○解析モード時のチェックファイルの判定のエンバグ修正
　　　1821+v11の履歴表示行数のバグ修正による事故でした

・Emuera1821+v11.1
　○REPEAT、FORの入れ子をエラー扱いから警告扱いに引き下げ
　　　個人的にはエラーでもいいが、現実的にはこっちの方が妥当かなーとか思い返してみる
　　　ついでに警告文に無限ループの指摘を追加

・Emuera1821+v11
　○履歴表示行数が設定を無視して10000行固定になっていたのを修正
　　　何でこんな中途半端なことしてたのやら…
　○#REFで定義された変数の想定仕様と内部挙動がかみあってないのを修正
　　　内部挙動の問題なのでコーディング上の影響はないと信じたい
　○v9,v9.1の追加警告まわりの色々tweak
　　　ちゃんとCOUNTの引数（定数の場合のみ）も見るように、eraMegaten母ちゃんあたりですごい誤爆してたので…
　　　後、多重化で引っかかったREPEATやFORに対して対応なし警告がセットになってしまうので、それを出ないようにちょっといじった

・Emuera1821+v10
　○STRJOINが第1引数が～～NAMEのような定数文字列配列の場合豪快に例外吐くのを修正
　　　この場合何が起こるか頭から抜けてた
　○v9、9.1の変更部分で特定条件で例外が飛ぶのを修正
　　　ただし、この例外が飛ぶ=その行がエラーを起こしている、なので、これが直ろうがその行は100%エラーです

・Emuera1821+v9.1
　○REPEATならびにCOUNTをループ変数にとるFORが多重になっている場合のエラー表示追加が引数をチェックしない場合豪快に例外吐くのを修正
　　　うん、この設定のこと頭から抜け落ちてた
　　　
・Emuera1821+v9
　○PRINTBUTTON系ならびにPRINTPLAIN系が特定条件でSETCOLORを反映しない問題を修正
　　　うん、これは設計が悪い、間違いなく悪い
　○REPEATならびにCOUNTをループ変数にとるFORが多重になっている場合のエラー表示追加
　　　関数跨ぎだけはさすがに無理です

・Emuera1821+v8
　○COPYCHARA、ADDCOPYCHARAがユーザー定義キャラクタ変数をコピーしない問題を修正
　　　作った後に増えた部分だから、Emuの人の作業漏れの公算大？

・Emuera1821+v7.2
　○文字列配列の結合関数STRJOINの追加
　　　書式：STRJOIN(＜配列変数＞{, ＜区切り文字列＞, ＜配列添え字開始位置＞, ＜配列添え字要素数＞})
　　　内容：ツールチップの最大表示時間を[表示時間(ms)]に設定
　　　引数：＜配列変数＞：結合した配列変数、キャラ変数を指定した場合エラーになるかも
　　　　　　＜区切り文字列＞：結合のさいに要素間に加える文字列、他の言語の同一処理同様、省略時は","が自動的に適用されます（区切り文字が必要ない場合は""を与えてください）
　　　　　　＜配列添え字開始位置＞、＜配列添え字要素数＞：指定した場合　配列添え字開始位置≦i＜開始位置＋配列添え字要素数　の範囲で結合する
　　　　　　後者を指定する場合、前者は省略不可
　　　最初JOINにしようと思ったけど事故がありそうだからこうした
　　　ver7.1で引数仕様変更
　　　ver7.2で数値配列に変数、区切り文字列の処理に問題があったので修正

・Emuera1821+v6
　○SORTCHARAをユーザー定義変数で行うと例外チュドーンになるのを修正
　　　このパターン想定しないコードで放置してたの、どこのEmuの人ですかー？

・Emuera1821+v5.2
　○@USERXXX系システム関数でREUSELASTLINEを利用したシステム内部処理渡しについて、
　　REUSELASTLINE命令の引数が空白でない場合、システム側メッセージの表示を行わないように変更
　　　ただし、これを利用してスクリプト側でREUSELASTLINEを用いてメッセージを表示させたい場合、
　　　システム内部処理とタイミングが異なる関係上、入力値の表示を自動でクリアできないため、
　　　入力値の表示を消したい場合、REUSELASTLINEの前にCLEARLINE 1を行う必要があります。

・Emuera1821+v5.1
　○v5の変更でSTRFORMの引数が文字列変数の場合の挙動がおかしくなったのを修正
　　　引数チェックのタイミングで挙動が変わるのは非常によろしくない
　○TOOLTIP_SETDURATIONが設定されているときのツールチップの表示位置のtweak
　　　マウスカーソルのやや下に表示されるように位置設定

・Emuera1821+v5
　○STRFORMが正しく振る舞わないバグを修正
　　　かなりクリティカルなバグというか、ここまで判明しなかったのが驚きというレベル

・Emuera1821+v4
　○一部命令のエラー時の表示がおかしいのを修正
　　　コピペしたら文面修正を忘れないのは大事な作業ですよ？
　○解析モード時はコンフィグによらず10000行までデータを保持するように
　　　一部バリアントはこれでも収まらないが、これ以上はメモリ不足との兼ね合いが生じ始めるためさすがにこれで許してください

・Emuera1821+v3.1
　○命令TOOLTIP_SETDURATIONの挙動に関するtweak
　　　表示命令の時間指定引数にwindows側の仕様でshortの最大値である32767より大きな値を渡すと無視されるようなので
　　　shortの最大値を超える場合は32767を引数とするように変更

・Emuera1821+v3
　○ツールチップの表示時間を設定する命令TOOLTIP_SETDURATIONを追加
　　　書式：TOOLTIP_SETDURATION [表示時間(ms)]
　　　内容：ツールチップの最大表示時間を[表示時間(ms)]に設定
　　　引数：[表示時間(ms)]：0以上の整数値　0の場合デフォルトの挙動になります、後タイマーの特性上極端に短い時間は想定通りに動かないかもねー

・Emuera1821+v2.1（ﾃｽﾄ向け公開）
　○再起動時、_fixed.configの変更に対して、コンフィグダイアログの固定状態が追随しないのを修正

・Emuera1821+v2
　○PRINT系命令の大半がSKIPDISPを無視するという実装漏れを修正
　　　CALLTRAIN使うとひどいことになりますね、はい

・Emuera1821+v1
　○1821+v2の修正がキャラクタ変数に適用されてないのを修正

作業従事者：妊）|дﾟ)の中の人
```

### Emuera.EM

```
◆ HTML_PRINTの<space>タグでparaに負数を指定できます

◆ プログラム動作中Resourceフォルダーの画像ファイルを常時占用しないようにしました

◆ INPUT, INPUTS, ONEINPUT, ONEINPUTS に第二引数追加(整数型，省略可，デフォルトは0)
   TINPUT, TINPUTS, TONEINPUT, TONEINPUTS に第五引数追加(整数型，省略可，デフォルトは0)
   追加引数==0時、または省略した時 本家版と同じです。
   追加引数!=0時 マウスクリックをエンターキーにみなす(RESULTSに空文字列を代入。ボタンを押した場合，ボタンのインデックスをRESULTS:1に代入)、左クリックの時RESULT:1を1、右クリックの時RESULT:1を2にします。また、同時にShift Ctrl Altを押した場合、そのキー状態をRESULT:2に保存します。(bit 17 18 19)

◆ ONEINPUT, ONEINPUTS, TONEINPUT, TONEINPUTS デフォルト値に2桁以上/2文字以上を指定できます

◆ LOADTEXT, SAVETEXT の第一引数が文字列の場合、第一引数をパスとしてファイルをロード/セーブします。Emuera.exeを相対パスで指定(".."は無効)。更に、configにで「LOADTEXTとSAVETEXTで使える拡張子」が決められた拡張子しか使えません。(デフォルトはtxtのみ)
   例文:LOADTEXTとSAVETEXTで使える拡張子:txt,(任意の拡張子),(任意の拡張子)

◆ int HTML_STRINGLEN html
   HTML_PRINTでhtmlを表示した結果の幅(半角文字単位)を返す、複数行がある場合1行目の幅を返す

◆ int HTML_SUBSTRING str, int
   strを2つに分ける
   例：
      HTML_SUBSTRING　"AB<b>CD</b>EFG",4
      PRINTSL RESULTS
      PRINTSL RESULTS:1
   結果：
      AB<b>C</b>
      <b>D</b>EFG
   (太字は普通より幅広いからです)

◆ int ISDEFINED str
   与えられた文字列と同名なマクロ(#DEFINE XXX)が定義されていたら1を返します。定義されていない場合0を返します。
◆ int EXISTVAR str
   与えられた文字列と同名な変数が定義されたら変数の性質によって正数を返します。定義されていない場合0を返します。
   変数が整数型の場合、返り値 setbit 1
   変数が文字列型の場合、返り値 setbit 2
   変数が定数の場合、返り値 setbit 3
   変数が2次元配列の場合、返り値 setbit 4
   変数が3次元配列の場合、返り値 setbit 5
   例：
      #DEFINE VAR_IS_NUM 1
      #DEFINE VAR_IS_STRING 1p1
      #DEFINE VAR_IS_CONST 1p2
      #DEFINE VAR_IS_2DARRAY 1p3
      #DEFINE VAR_IS_3DARRAY 1p4

      #DIM FOO,10,10

      ...

      IF EXISTVAR("FOO")==(VAR_IS_NUM|VAR_IS_2DARRAY)
         PRINTL TRUE
      ELSE
         PRINTL FALSE
      ENDIF
      IF EXISTVAR("FOO2")
         PRINTL TRUE
      ELSE
         PRINTL FALSE
      ENDIF
   結果：
      TRUE
      FALSE

◆ int ENUMFUNCBEGINSWITH str
   int ENUMVARBEGINSWITH str
   int ENUMMACROBEGINSWITH str
   int ENUMFUNCENDSWITH str
   int ENUMVARENDSWITH str
   int ENUMMACROENDSWITH str
   int ENUMFUNCWITH str
   int ENUMVARWITH str
   int ENUMMACROWITH str
   定義されたシンボルを返す，結果をRESULTSに代入，総数を返します。
   FUNCは関数名，VARは変数名，MACROはマクロ名。
   BEGINSWITHはstrで始まるシンボルです。
   ENDSWITHはstrで終わるシンボルです。
   WITHはstrを含むシンボルです。

◆ int GETVAR str
   str GETVARS str
   int(1) SETVAR str, value
   文字列で表した変数のGET、SET関数です。
   例：
      #DIM FOO = 10,10

      SETVAR "FOO:1", 5
      PRINTFORML {FOO} {FOO:1} {GETVAR("FOO:1")}
   結果：
      10 5 5

◆ int(1) VARSETEX str, value(, int, int, int)
   VARSETと似ています，第1引数は変数名の代わりに文字列で表した変数名に変更します
   第3引数は0以外または省略した場合、配列の全てにvalueを代入、そうでない場合，最低次元の配列に代入
   例：
      #DIM FOO,2,3

      VARSETEX "FOO:1:1", 5, 0
      PRINTFORML {FOO:1:0} {FOO:1:2}
      VARSETEX "FOO:1:0", 10
      PRINTFORML {FOO:0:0} {FOO:1:2}
   結果：
      0 5
      10 10

◆ int(1) ARRAYMSORTEX str/intArray, strArray(, int)
   ARRAYMSORTと似ています，第1引数は文字列の場合，変数名の代わりに文字列で表した変数配列名に変更します
   第2引数は文字列配列，それぞれの要素は並び替えたい配列の変数名を表す
   第2引数は0以外または省略した場合正順、0の場合逆順
   例：
      #DIM IDX = 4,2,3,1
      #DIM AA = 1,2,3,4
      #DIM BB = 5,3,1,2
      #DIMS VARS = "IDX", "AA", "BB" ; IDXを入れないとIDXを並び替えしない

      ARRAYMSORTEX IDX, VARS      ;
      PRINTFORML > IDX == {IDX},{IDX:1},{IDX:2},{IDX:3}
      PRINTFORML > AA == {AA},{AA:1},{AA:2},{AA:3}
      PRINTFORML > BB == {BB},{BB:1},{BB:2},{BB:3}
      PRINTL
      ARRAYMSORTEX "IDX", VARS, 0   ;逆順
      PRINTFORML > IDX == {IDX},{IDX:1},{IDX:2},{IDX:3}
      PRINTFORML > AA == {AA},{AA:1},{AA:2},{AA:3}
      PRINTFORML > BB == {BB},{BB:1},{BB:2},{BB:3}

   結果:
      > IDX == 1,2,3,4
      > AA == 4,2,3,1
      > BB == 2,3,1,5

      > IDX == 4,3,2,1
      > AA == 1,3,2,4
      > BB == 5,1,3,2

◆ int REGEXPMATCH str, patter
   strが正規表現パターンpatterに合うなら1を返す，そうでない場合0を返す

◆ int XML_DOCUMENT id, xml
   xmlを解析し、XmlDocumentで保存する。1を返す
   id(整数型)に対応するXmlDocumentがすでに存在している場合，0を返す

◆ int XML_RELEASE id
   id(整数型)に対応するXmlDocumentを削除する。1を返す

◆ int XML_EXIST id
   id(整数型)に対応するXmlDocumentが存在してい場合1を返す，そうでない場合0を返す

◆ int XML_GET xml, xpath(, strArray/int, int)
   xpath(文字列型)の規則でxmlからノードを選択し、結果数を返す
   第1引数は整数の場合，それをIDにみなして保存したXmlDocumentを使う。そのIDに対応するXmlDocumentが存在していない場合，-1を返す
   第3引数は0以外または省略した場合，総数のみ返す，他の整数の場合，RESULTS結果を返す
   第3引数は文字列配列の場合，その配列に結果代入
   第4引数は結果のタイプを決めます
   1: InnerText
   2: InnerXml
   3: OuterXml
   他: Value
   保存したXmlDocumentが存在しない場合，-1を返す

◆ int XML_SET xml, xpath, str(, setall, int)
   xpath(文字列型)の規則でxmlからノードを選択し、strを代入します。ノードの合致結果の数を返す
   第1引数は整数の場合，それをIDにみなして保存したXmlDocumentを使う。そのIDに対応するXmlDocumentが存在していない場合，-1を返す
   第4引数(整数型)は0または省略した場合，一つ以上の合致結果に対して，代入を行わない
   第5引数は代入のタイプを決めます
   1: InnerText
   2: InnerXml
   他: Value
   保存したXmlDocumentが存在しない場合，-1を返す

◆ str XML_TOSTR int
   第一引数で指定したXmlDocumentを文字列に変換して返す。
   X保存したmlDocumentが存在しない場合，空文字列を返す

◆ int XML_ADDNODE xml, xpath, xml(, addmethod, setall)
   第1引数は整数の場合，それをIDにみなして保存したXmlDocumentを使う。そうでない場合，第1引数が文字列変数でなければならない
   第1引数で指定したXMLに対して:
   addmethodが0または省略した場合，xpath(文字列型)で選択した要素ノードを親ノードとし、その子ノードリストの最後にxmlを子ノードとして追加する
   addmethodが1の場合，xpath(文字列型)の規則で選択した要素ノード(ルートノードではない)を子ノードとし、そのノードの前にxmlを兄弟ノードとして追加する
   addmethodが2の場合，xpath(文字列型)の規則で選択した要素ノード(ルートノードではない)を子ノードとし、そのノードの後にxmlを兄弟ノードとして追加する
   setallが0または省略した場合，一つ以上の合致結果に対して，追加を行わない
   成功した場合，合致結果の数を返す。失敗した場合，0を返す。保存したXmlDocumentが存在しない場合，-1を返す

◆ int XML_REMOVENODE xml, xpath(, setall)
   第1引数は整数の場合，それをIDにみなして保存したXmlDocumentを使う。そうでない場合，第1引数が文字列変数でなければならない
   第1引数で指定したXMLに対して，xpathで選択した要素ノード(ルートノードは無効)を削除する。
   setallが0または省略した場合，一つ以上の合致結果に対して，削除を行わない
   成功した場合，合致結果の数を返す。失敗した場合，0を返す。保存したXmlDocumentが存在しない場合，-1を返す

◆ <1> int XML_REPLACE xml, xml2
   <2> int XML_REPLACE xml, xpath, xml2(, setall)
   第1引数は整数の場合，それをIDにみなして保存したXmlDocumentを使う。そうでない場合，第1引数が文字列変数でなければならない
   バリアント<1>の第1引数は整数だけ有効となる
   <1> 保存したXmlDocumentの内容をxml2で上書きする
   <2> xmlに対し、xpath(文字列型)の規則で選択した要素ノード(ルートノードではない)をxml2で上書きする
   成功した場合，合致結果の数を返す。失敗した場合，0を返す。保存したXmlDocumentが存在しない場合，-1を返す

◆ int XML_ADDATTRIBUTE xml, xpath, name(, value, addmethod, setall)
   第1引数は整数の場合，それをIDにみなして保存したXmlDocumentを使う。そうでない場合，第1引数が文字列変数でなければならない
   第1引数で指定したXMLに対して:
   addmethodが0または省略した場合，xpath(文字列型)で選択した要素ノードの属性リストの最後に、属性名name(文字列型)=属性の値value(文字列型)の属性を追加する
   addmethodが1の場合，xpath(文字列型)の規則で選択した属性の前に、属性名name(文字列型)=属性の値value(文字列型)を兄弟属性として追加する。valueは省略可
   addmethodが2の場合，xpath(文字列型)の規則で選択した属性の後に、属性名name(文字列型)=属性の値value(文字列型)を兄弟属性として追加する。valueは省略可
   setallが0または省略した場合，一つ以上の合致結果に対して，追加を行わない
   成功した場合，合致結果の数を返す。失敗した場合，0を返す。保存したXmlDocumentが存在しない場合，-1を返す

◆ int XML_REMOVEATTRIBUTE xml, xpath(, setall)
   第1引数は整数の場合，それをIDにみなして保存したXmlDocumentを使う。そうでない場合，第1引数が文字列変数でなければならない
   第1引数で指定したXMLに対して，xpathで選択した属性を削除する。
   setallが0または省略した場合，一つ以上の合致結果に対して，削除を行わない
   成功した場合，合致結果の数を返す。失敗した場合，0を返す。保存したXmlDocumentが存在しない場合，-1を返す

◆ int EXISTFILE str
   第一引数をパスとしてファイルの存否をチェックします。Emuera.exeを相対パスで指定(".."は無効)。
   存在しているなら1を返す，そうでない場合0を返す

◆ int MAP_CREATE str
   第一引数で指定した連想配列(Dictionary<string, string>)を作ります。1を返す
   すでに存在している場合0を返す

◆ int MAP_EXIST str
   第一引数で指定した連想配列(Dictionary<string, string>)の存否をチェックします
   存在しているなら1を返す，そうでない場合0を返す

◆ int MAP_RELEASE str
   第一引数で指定した連想配列(Dictionary<string, string>)を削除します。1を返す

◆ str MAP_GET str, str
   第一引数で指定した連想配列(Dictionary<string, string>)に，第二引数のキーが存在する場合，その値を返す。
   存在していない場合，または連想配列自体が存在しない場合も，空文字列を返す
   (例外発生しないので必要があればMAP_HASで確認してください)

◆ int MAP_HAS str, str
   第一引数で指定した連想配列(Dictionary<string, string>)に，第二引数のキーの存否をチェックします
   存在しているなら1を返す，そうでない場合0を返すください
   連想配列自体が存在しない場合，-1を返す

◆ int MAP_SET str, str, str
   第一引数で指定した連想配列(Dictionary<string, string>)に，第二引数のキーとペアした値に第三引数を代入
   キーが存在していない場合キーを作る。1を返す。
   連想配列自体が存在しない場合，-1を返す

◆ int MAP_REMOVE str, str
   第一引数で指定した連想配列(Dictionary<string, string>)に，第二引数のキーとペアした値を削除する。1を返す。
   連想配列自体が存在しない場合，-1を返す

◆ int MAP_SIZE str
   第一引数で指定した連想配列(Dictionary<string, string>)に含まったキー-値ペアを返す
   連想配列自体が存在しない場合，-1を返す

◆ int MAP_CLEAR str
   第一引数で指定した連想配列(Dictionary<string, string>)に含まったキー-値ペアを全部削除する。1を返す
   連想配列自体が存在しない場合，-1を返す

◆ <1> str MAP_GETKEYS str
   <2> str MAP_GETKEYS str, int
   <3> str MAP_GETKEYS str, strArray, int
   第一引数で指定した連想配列(Dictionary<string, string>)に:
   <1> "キー1,キー2,キー3,..."のような形の文字列を返す
   <2> 第二引数が0ではない場合，RESULTSにキーを順次代入。RESULTS:0を返す
   <3> 第三引数が0ではない場合，第二引数が指定した文字列配列変数にキーを順次代入。空文字列を返す
   連想配列自体が存在しない場合，空文字列を返す
   キーの数をRESULTに代入

◆ str MAP_TOXML str
   第一引数で指定した連想配列(Dictionary<string, string>)をXML文字列に変換し，XML文字列を返す。
   連想配列自体が存在しない場合，空文字列を返す

   返したXMLは
   <map>
      <p><k>キー1</k><v>値1</v></p>
      <p><k>キー2</k><v>値2</v></p>
      <p><k>キー3</k><v>値3</v></p>
      ....
   </map>
   のような形です

◆ int MAP_FROMXML str, str
   第一引数で指定した連想配列(Dictionary<string, string>)に第二引数のXML文字列からキー-値ペアを読み取る。
   キーが存在している場合その値を上書きします。1を返す
   連想配列自体が存在しない場合，0を返す

   XMLは、必ず
   <map>
      <p><k>キー1</k><v>値1</v></p>
      <p><k>キー2</k><v>値2</v></p>
      <p><k>キー3</k><v>値3</v></p>
      ....
   </map>
   のようにしないといけません
```

### Emuera-Anchor

```
================== How to create proper translation of era Games using Anchor ===================

Current version: 1.24

# Changes from version 1.24
- Added TR_NAME commands which returns

# Changes from version 1.19
- STR_TR.csv now translating properly
- Added support for translating characters using TRCHARA file, see 2
- Multiple bug fixes and functions added since v1.10

# Changes from version 1.10
- Added comment support

# Changes from version 1.09
- Most translations will now work without putting it in ALL_TR.csv
- This should be the final version of where you need to place the translation for it to work.

================================ 1 - CSV Translation System (TR) ================================

You can now translate variable names without doing mass replaces for CSV.
To translate the printing of a variable you need to:

1) Create the file XXXXX_TR.csv where XXXXX is where the variable is named like Base_TR.csv or Talent_TR.csv if it doesn't already exist.
2) Put originalName,newName alone on a line.

Everything after ; is a comment.

; Example of lines in TR files.
Male,Trap; This part is a comment.
LargeMirror,Large mirror



Only these files can have a XXXX_TR.csv version.
{"Abl", "Talent", "Exp", "Mark", "Palam", "Item","Train", "Base","Source", "Ex", "EQUIP", "TEQUIP", "Flag", "TFLAG", "Cflag", "Tcvar", "CSTR", "Stain","Cdflag1", "Cdflag2", "Str", "TSTR","SaveStr", "GLOBAL", "GLOBALS", "ALL"}

-- In case the original name stills shows --

The name is sometimes written directly in a print, to mass replace it without touching the variable please use this regular expression: (?<=Print.*|SET_PICTURE.*)(?<!:)\b(XXX)\b(?!"). It is possible that it will miss rare occurrence but it will not touch variables.
That said, it's preferable to do things by hand still.


-- In the case it's still not working. Create ALL_TR.csv if it's not already --
(Warning: This feature may be phased out in the future)

It replace directly what is between %% in a PRINTFORM.

Put originalName,newName alone on a line.  (Same things as the others _TR for now)
Male,Trap; This part is a comment
LargeMirror,Large mirror

========================== 2 - Character Translation System (TRCHARA) ===========================

In order to translate character files and their contents, you need to use a system different from the above.

1) Create a file named TRCHARA.csv inside the root CSV one or any of its sub-folders, this will contain the translated content.
	1b) As long as the filename starts with 'TRCHARA' (all caps) and ends with '.csv', it may have any name.
		"TRCHARA(キミキス).csv" for example is a valid TRCHARA filename.
	1a) For better organization, you can create multiple TRCHARA.csv files, each having different character translation data.
		Better yet if you name each differently

2) Add translation data for each character in the TRCHARA file, as follows:
	2a) First start with "NO,XXX" where XXX is the character number on the character csv file.
		It'll accept "番号,XXX" as well, if you prefer to copy&paste from the original file.
	2b) From here on, all steps are optional, add just what you wish to translate, each on a single line of text:
		"NAME,Full Translated Name," or "名前,Full Translated Name,"
	2c) "CALLNAME,Nickname," or "呼び名,Nickname,". Remember, there's an "," at the end.
	2d) "NICKNAME,Nickname," or "あだ名,Nickname" <-- only add if it あだ名 exists on the original file
		"MASTERNAME,How they adress their master," or "主人の呼び方,Name of Master," <-- only add if 主人の呼び方 is present on the original file
	2e) "CSTR,XX,Character string to be translated". Do note there's no "," on the end of the line of CSTR data. The XX number must be equal to the original number.
	2f) When the translation data for that character is complete, add a new line with "ENDCHR,XXX," with XXX as the number of the character
		You may also use "ENDCHR,," to the same effect, but the above is better for organization purposes

3) Add a new empty line and then go back to 2 for each new character translation added.
4) Save the file, check for errors, save again.

If correct, Emuera-Anchor should correctly translate the character data once a new game has been started.

==================================== 3 - TR-Related Commands ====================================

str TR_NAME (string code, int index)

	This command returns the translated (if it can't, returns the original) string based on code (ABL,TALENT,etc) and a index.
	The intended usage for this is when you need to manipulate translated strings, as Anchor will not translate things unless it's meant to be printed. An example would be working with CSTRs and the like, where they'll be saved into the save file and loaded again later, and Anchor won't be able to translate it.
	As some scripts may make use of CSTRs, you as the scripter should make sure to deal with the problems that could arise from using TR_NAME before publishing your changes.
	Also note that TRCHARA translated strings are always translated due to the nature of the beast, so there's no need for a character-equivalent command to exist.
	Some valid uses as follows:
		PRINTFORML %TR_NAME(ABL,"感性")%
		CSTR:99 = The string %TR_NAME("ABL",25)% is a translation of 感性

If you have any bug reports or comments or it just doesn't work @Bartoum or @JVN on the discord channel or post in the /hgg/ thread.
```