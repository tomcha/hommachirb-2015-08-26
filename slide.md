## Rubyとコンピューター将棋
  第6回 hommachirb
___
#### 自己紹介

- 名前 tomcha
- twitter @tomcha_
- blog Perlがくしゅう帳(Rubyも)
![photo](image/twitter_icon_mini.png)

___
#### エンジニアのお祭りYAPC::Asia 2015

[http://yapcasia.org/2015/](http://yapcasia.org/2015/)

詳しくは技術評論社のYAPC::Asia Tokyo 2015 スペシャルレポートで

[http://gihyo.jp/news/report/01/yapcasia2015](http://gihyo.jp/news/report/01/yapcasia2015)
___
#### 宣伝

- 9/12(土)グランフロント大阪内 ナレッジサロンで「なにわPerl」というもくもく会やります。
- 参加者募集中です。
- 詳しくはDoorKeeper「 なにわPerl」で検索
- あと、「Perl入学式」というプログラミング初心者向け勉強会もサポーターで参加しています。
___
#### 今日の内容
- コンピューター将棋について
- Rubyで将棋対戦サーバーに通信するには
- socket ライブラリ、TCPSocketクラス

---
#### 将棋好きですか？

___
#### 将棋のルール
- 2人で対戦するゲーム。
- 9*9の81マスにそれぞれ20個の駒を持ってスタート。
- 交互に駒を動かして、相手の「王」の駒を先に取れば勝ち。
- 将棋のプロがいる。（有名なプロ棋士 羽生善治さん）

___
#### 将棋盤
![syougi](image/shougi.jpg)

___
#### 電王戦

![denousen](image/denousen_logo.jpg)

___
- 2012年 第1回 コンピューターの勝ち
- 2013年 第2回 コンピューターの3勝1敗1引き分けプロ棋士(1勝) コンピューター（3勝）1引き分け
- 2014年 第3回 コンピューターの4勝1敗
- 2015年 Final コンピューター2勝3敗
- ※2013年から5vs5の団体戦、2014年よりソフトの事前貸出

___
#### コンピュータが考えた指し手をロボットが指す！
![denoutesan](image/denoutesan.jpg)

___
#### 電王戦のようす
![denoutesan](image/denou_2.jpg)

___
#### コンピューター将棋の歴史は古く、電王戦よりも前から。
- コンピュータ将棋協会(CSA)
  - 世界コンピュータ将棋選手権
    - 第1回は 1990年12月
    - 以降、年1回のペースで開催
    - 近年はGWに開催されている。
    - 仕様は公開されている。
    - [CSAサーバ プロトコル ver.1.2.1](http://www.computer-shogi.org/protocol/tcp_ip_client_121.html)

___
#### インターネット上の対戦サーバ
**Floodgate**

[http://wdoor.c.u-tokyo.ac.jp/shogi/](http://wdoor.c.u-tokyo.ac.jp/shogi/)

___
#### ローカル環境で動く対戦サーバ
  
- ローカル環境で立ち上げることができるサーバが公開されている。**Rubyで書かれている。**
- [http://shogi-server.osdn.jp/](http://shogi-server.osdn.jp/)
- ruby 1.8 系統を用意する。
- 自分は、手元で2.0系や2.1系で動かしている。

___
自分でクライアントを作って接続してみる

接続イメージ
![kousei](image/kousei.jpg)

___
#### プロトコルとは？

**IT用語辞典**  
  
プロトコルとは、手順、手続き、外交儀礼、議定書、協定などの意味を持つ英単語。通信におけるプロトコルとは、複数の主体が滞りなく信号やデータ、情報を相互に伝送できるよう、あらかじめ決められた約束事や手順の集合のこと。
  
要するに手順のおやくそくのこと。

___
#### それって吉本新喜劇やん！
![yoshimoto](image/kuwabara.jpg)

___
```
 Client >> ごめんください。  
 Client >> どなたですか。  
 Client >> 桑原和男が参りました。  
 Client >> お入りください、ありがとう。  
 Server >> (一同コケる)  
 　
 お芝居にログイン  
```

___
#### [CSAサーバプロトコルver1.2.1](http://www.computer-shogi.org/protocol/tcp_ip_server_121.html)
```
ログイン (C -> S)
LOGIN <username> <password>
  
認証成功 (S -> C)
LOGIN:<username> OK
  
対局に参加 (C -> S)
AGREE <GameID>
  
対局開始 (S -> C)
START:<GameID>
```

___
#### 手番の時の指し手のやりとり
```
指し手 (C -> S)
+7776FU
指し手が合法のメッセージ S to C
+7776FU,T12
```

---
#### Rubyで通信するライブラリ

- socketライブラリ
  - ソケットとは・・・要は出入口の事。クライアントを作る時は、このsocketライブラリで定義されているクラスのオブジェクトを介してテキストをやりとりする。

___
#### TCPとUDP（ざっくり）

- TCPは内容が保証される通信方法。そのかわり手順が煩雑。
  - ストリーム
- UDPは内容が保証されない通信方法。送りっぱなしだが、早い。
  - データグラム

___
#### TCPSocketクラス

- クラスの継承リスト: TCPSocket < IPSocket < BasicSocket < IO < Enumerable < File::Constants < Object < Kernel
  - open又はnewメソッド・・・ホスト名、ポートを渡すとオブジェクトが返ってくる。(TCPSocketクラス)
  - writeメソッド・・・引数の文字列を（サーバーへ）出力する。(IOクラス)
  - getメソッド・・・送られてきた文字列を取得する。(IOクラス)
  - [実際に書いたコード](https://github.com/tomcha/shogi/blob/master/agent.rb)

___
#### newメソッド
```
sock = TCPSocket.open("server_name", 4082)
```

___
#### writeメソッド
```
sock.write("output text\n")
```

___
#### getsメソッド
```
while(c = sock.gets.chomp!)
  //得たテキストを何らかの処理など 
  //puts c など
end
```

___
時間があればデモします。

---
Let's enjoy programing!

