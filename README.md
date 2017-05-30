# 日本語 漢字置換インプット メソッド for IBus

漢字置換インプット メソッドは、FedoraやUbuntuなどIBusに対応したオペレーティング システムで日本語を入力するためのインプット メソッドです。漢字置換インプット メソッドは、約1,200行(v0.1.0)のプログラムで、すべてプログラミング言語Pythonで記述されています。

## 漢字置換インプット メソッドについて

漢字置換インプット メソッドでは、タイプしたひらがなはタイプライターのように本文中にそのまま直接入力されていきます。ひらがなを入力するために、〔確定〕キーや〔無変換〕キーなどをおす必要はありません。

漢字やカタカナのまじった「しろうとむけワープロの登場を期待したいですね。」(梅棹さん，2015)という文を入力したいときは、つぎのようにタイプします。

    しろうとむけわーぷろ〔変換〕のとうじょう〔変換〕をきたい〔変換〕したいですね。

このようにカタカナや漢字にしたい部分の直後で〔変換〕キーをおすと、ひらがなからカタカナや漢字に置換できます。ひらがなで文を最後まで入力したあとで、カーソルをまえにもどして〔変換〕キーをおしてもかまいません。

※ 文節を意識して変換する必要がないので、文節という概念をまだならわない小学生にも、つかいやすい方式になっています。

### 梅棹式表記

梅棹忠夫さん(1920-2010)は、同音異義語のような目のことばをさけて、耳できいただけで意味がわかるような、わかりやすい日本語を著述のなかでつかわれていました。その表記法は「梅棹式」とよばれることがあります。梅棹式表記法の原則(梅棹さん，2004)は、つぎの2つです。

* 和語はかなでかく。1音の動詞で意味が判別しにくいときは、漢字の使用も許容する(例:切る、着る)。
* 漢語は漢字でかく。固有名詞のほかは、常用漢字を意識してむずかしい漢字はさける。

漢字置換インプット メソッドは、 この「梅棹式」で日本語を入力しやすいように開発をすすめてきた日本語インプット メソッドです。付属している辞書も梅棹式表記にあわせて、使用する漢字は固有名詞をのぞくと常用漢字とその音訓の範囲に限定してあります。

※ このREADMEファイルも、漢字置換インプット メソッドをつかって梅棹式表記でかかれています。

## インストール方法

漢字置換インプット メソッドは、ソースコードから下記の手順でインストールしてください。

    ./autogen.sh
    make
    sudo make install
    ibus restart

上記の手順ができたら、IBus Preferencesの"Input Method"タブから、

![アイコン](icons/ibus-replace-with-kanji.png) Japanese - ReplaceWithKanji

を選択してください。

**参考**: Fedoraではtarballをつくってから、以下のようにしてrpmファイルをつくることもできます。

    $ make dist
    $ rpmbuild -ta ibus-replace-with-kanji-#.#.#.tar.gz

※ ビルドするとき、Python3とIBusのほかに、Fedoraではibus-develが、Ubuntuではlibibus-1.0-devなどが必要になります。

    $ sudo dnf install ibus-devel # Fedoraの場合
    $ sudo apt-get install libibus-1.0-dev # Ubuntuの場合

## キー操作

キーの操作は下記のようになっています。なお、漢字置換インプット メソッドでは、 誤変換はテキスト エディターやワープロのUndo機能をつかって、もとのひらがなまでもどすことができます(**モードレス方式**)。

キー | 内容
------------ | -------------
〔Caps Lock〕 | 英数/かなモードのきりかえ
〔変換〕(JISキーボードの場合) <br>〔スペース〕(USキーボードの場合) | 置換変換開始，次候補
〔↓〕 | 次候補
〔無変換〕(JISキーボードの場合) <br>〔Shift〕-〔スペース〕(USキーボードの場合) <br>〔↑〕 | 前候補
〔Shift〕-〔→〕 | よみを短縮する(下記参照)
〔Enter〕 | 同音異義語の確定(つづきを入力すれば〔Enter〕をおさなくても自動的に確定します)
〔Esc〕 | 同音異義語の選択とりけし(ひらがなにもどす)<br>入力しかけのローマ字のとりけし
〔Page Up〕，〔Page Down〕 | 同音異義語ウィンドウのページのきりかえ
〔カタカナ〕(JISキーボードの場合)<br>右〔Shift〕(英語キーボードの場合)<br>右〔Ctrl〕(親指シフトおよびTRONかな配列の場合) | カタカナ置換(カーソルの直前のひらがなを1文字ずつてまえにカタカナに置換していきます)
〔Alt〕-〔カタカナ〕(JISキーボードの場合)<br>〔Alt〕-右〔Shift〕(英語キーボードの場合)<br>〔Alt〕-右〔Ctrl〕(親指シフトおよびTRONかな配列の場合) | カタカナ入力モード(変換操作でひらがなモードにもどります)

* 同音異義語ウィンドウは置換候補が複数ある場合にだけ表示されます。
* 同音異義語ウィンドウから候補を選択するときに、数字キーには対応していません(数字をうつと、そのまま数字が入力されます)。
* 辞書に登録のないカタカナ語は、カタカナ置換をつかうか、カタカナ入力モードにきりかえて入力します。
* 辞書の学習結果は、~/.local/share/ibus-replace-with-kanji/ディレクトリの中に保存されます。

### よみの短縮

※ よみを短縮するときのキー操作は、一般的なワープロで選択範囲を短縮するときの操作とおなじです。

漢字置換インプット メソッドでは、〔変換〕キーをおすと、そのときのカーソル位置の直前のひらがなの文字列が、漢字やカタカナに置換されます。そのとき、辞書ファイルの中からは、よみが一致する一番よみのながい単語がえらばれます。そのため「生きがい論」と入力したい場合、

    生きがいろん〔変換〕

とタイプすると、よみとして「がいろん」がえらばれて、

    生き概論

のようになります。そこで〔Shift〕-〔→〕をおすと、よみが「いろん」に短縮されて、

    生きが異論

とかわります。さらに、もう1度、〔Shift〕-〔→〕をおすと、よみが「ろん」に短縮されて、

    生きがい論

と目的の「生きがい論」に置換できます。

### 和語の動詞や用言の漢字での入力方法

#### 和語の1音の動詞の漢字

漢字置換インプット メソッドでは、「切る」と「着る」のように、1音で意味が判別しにくい動詞は、キーボードから直接漢字を入力することができるようにしてあります(**漢直方式**) 。

直接入力できる漢字は、下記の表のA，B，C列の漢字です。

音 | A | B | C | かな優先
-- | -- | -- | -- | --
い | 射 \i | 生 \xi | | (入)
う | 売 \u | | | (得)
え | 得 \e | | | (獲)
お | 負 \o | 織 \xo | | (追)，(折)
か | 買 \ka | 飼 \ga | 欠 \xka ，く | (書)
き | 着 \ki | 切 \gi
た | 裁 \ta | | |(断)
と | 解 \to | 説 \do
に | 似 \ni | 煮 \xni
\ | ￥ \ \ |

ローマ字入力の場合は表のように、〔\〕キー を前置キーとして、


* \ki → 着
* \gi → 切

といった具合にタイプすると、対応する漢字を直接入力できます。

かな入力の場合、 A，B，C列の漢字は、〔\〕キー もしくは〔半角/全角〕キーにつづけて、それぞれつぎのようにタイプして入力します。

* A列 音のまま、かなをタイプ
* B列 音とおなじキーのシフト側(Aがシフト側ならノーマル側)をタイプ
* C列 表のかなをタイプ

たとえば、動詞の音が「き」であれば、

* \ き → 着
* \ 〔Shift〕-き → 切

といった具合に漢字を直接入力できます。

**補足**:  これらの動詞は、よみから漢字に変換する方法では、誤変換になりやすいようです。梅棹式表記では、「生む」と「産む」ようにもともとの和語がおなじものは、漢字でかきわけずに、そのままひらがなで「うむ」とかきます。そのため、こうした意味の判別しにくい1音の動詞はそれほどおおくなく、上記の表のような漢字に限定されるようです。

#### そのほかの和語の用言の漢字

そのほかの和語の用言も漢字でかきたい場面がまったくないわけではありません。そういった場合は、おくりがなをつける部分で〔\〕キーもしくはもしくは〔半角/全角〕キーをタイプしてから〔変換〕キーをおすと、漢字に置換することができます。

例)

    おく\〔変換〕 → 送，贈，遅，後

〔\〕キーはおくりがなをつける直前の部分でタイプしますが、その位置はおくりがなの原則どおりである必要があります。たとえば、「行なう」はただしいおくりがなのつけかたの1つですが、これを入力するときは、

    おこな\〔変換〕なう
    
のように「な」 を2回入力する必要があります。

※ この方法で置換できるのは、常用漢字表に記載がある漢字にかぎられます。

### キーボード配列の選択

下記のキーボード配列に対応しています。ファイル名に.109がふくまれているものは、JISキーボード用です。設定はJSONで記述しています。通常のテキスト エディターをつかってあたらしい配列を定義してつかうこともできます。

キーボード配列 | 設定ファイル名 | 補足
------------ | ------------- | -------------
99式ローマ字 | roomazi.json<br>roomazi.109.json | デフォルト。99式ローマ字は梅棹忠夫さんが会長をつとめられた日本ローマ字会でさだめられた方式です。基本部分は訓令式とおなじです。
JISかな配列 | jis.109.json |
[ニュー スティックニー配列](https://github.com/esrille/new-stickney) | new_stickney.json | まだローマ字の習得がむずかしい小学生にもおぼえやすいような、かな配列として研究中の配列です。
TRONかな配列 | tron.json | エスリルNISSE用。
NICOLA(親指シフト) | nicola.json<br>nicola_f.json | エスリルNISSE用。

設定を変更するときは、IBus configの、

    engine/replace-with-kanji-python:layout

に上記の設定ファイル名をフルパス名で指定してください。

例 (インストール先が /usr/local の場合) :

    /usr/local/share/ibus-replace-with-kanji/layouts/roomazi.json

IBus configの設定は、dconf Editorをつかう場合は、

    /desktop/ibus

から変更できます。設定を変更すると、自動的に漢字置換インプット メソッドに反映されます。

※ エスリルのNISSEをつかわれている場合は、キーボードをpcモードに設定して、~/.Xmodmapを以下のように設定しておくと、FNキーで〔英数〕と〔かな〕のモードきりかえができるようになります。またNISSE側のかな配列は、TRONかな配列などをつかう場合でもローマ字に設定しておいてください(漢字置換インプット メソッド側で各配列に対応しています)。

    keycode 191 = F13 F13 F13
    keycode 192 = F14 F14 F14

## 辞書ファイルについて

### 個人用辞書

標準の辞書ファイルに不足している単語があった場合は、~/.local/share/ibus-replace-with-kanji/ディレクトリの中の'my.dic'という名前のファイルの中に単語を追加できます。追加した単語は、次回IBusデーモンを起動したときから、つかえるようになります。IBusデーモンは、コマンド ラインからは、

    ibus restart

で再起動できます。辞書ファイルには、以下のようなテキスト形式で単語を保存します。

    ; コメント
    ; よみ /単語/
    きれい /綺麗/
    ; よみ /単語1/単語2/
    こーひー /珈琲/咖啡/
    ; ようげん― /おくりがなの手前まで/
    ささや― /囁/

### システム辞書

付属の漢字辞書ファイル(restrained.dic)およびカタカナ語辞書ファイル(katakana.dic)は、それぞれ[SKK辞書](http://openlab.ring.gr.jp/skk/dic-ja.html)および[EDICT辞書](http://www.edrdg.org/jmdict/edict.html)から、漢字置換インプット メソッド用にプログラムで自動生成しています。

辞書生成用のプログラムはソース ディレクトリのdic_toolsサブ ディレクトリの中にあります。付属している辞書の生成手順は、下記のとおりです。

    # 漢字辞書
    ./restrain.py /usr/share/skk/SKK-JISYO.ML > restrained.dic
    
    # カタカナ語辞書
    ./katakana.py edict2 > katakana.dic

※ SKK辞書はSKK(ibus-skk等)をインストールするとあわせてインストールされます。edict2は配付元から入手してください。

## 制限事項

* 置換変換はIBusの周辺テキストAPIをつかって実現しています。周辺テキストAPIに未対応あるいは完全には対応できていないアプリケーションでは、直前に入力した文字列にたいしてのみ置換変換をすることができます(カーソルを移動してあとから置換変換することはできなくなります)。GNOMEの標準のテキスト エディターgeditなどは周辺テキストAPIにきちんと対応しているようです。

## 参考文献

* 『[日本語と事務革命](https://www.amazon.co.jp/dp/4062923386)』，梅棹忠夫，講談社学術文庫，2015.
* 「漢字はやめたい」，『[日本語の将来](https://www.amazon.co.jp/dp/4140910011)』， 梅棹忠夫，NHKブックス，2004.
* [『日本語と事務革命』— 梅棹式表記法のための日本語入力システムをかんがえる](http://shiki.esrille.com/2017/04/blog-post.html)，2017.
* [梅棹式表記法のための日本語入力システムをかんがえる(その2)](http://shiki.esrille.com/2017/04/2.html)，2017.
