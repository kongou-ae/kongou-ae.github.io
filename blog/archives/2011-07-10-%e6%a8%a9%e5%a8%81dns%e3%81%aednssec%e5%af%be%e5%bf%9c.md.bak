---
title: DNSSEC始めました。
author: kongou_ae
layout: post
date: 2011-07-09
url: /blog/archives/266
categories:
  - DNS
---
　「DNSの勉強をしよう」という事で構築したaimless.jpの権威DNSを、DNSSECに対応させました。これで、権威DNSはIPv4/v6のデュアルスタック＆DNSSECです。実施にあたっては、下記の資料を参考にしました。

<a href="http://www.nic.ad.jp/ja/materials/iw/2009/proceedings/h3/iw2009-h3-01.pdf" target="_blank">・DNSSECチュートリアル～実践編～</a>
  
<a href="http://yebo-blog.blogspot.com/2007/01/dnssec-3.html" target="_blank">・DNSSECの勉強 (3)</a></li> 

### 鍵の作成

　まずは署名に必要な鍵を作成します。DNSSECで必要な鍵は、ゾーンファイルを署名する為のZSKと、ZSK公開鍵を署名する為のKSKです。ZSKは「Zone Signing Key」の略称、KSKは「Key signing key」の略称ですが、個人的には「Zone syomei kagi」「Kagi syomei kagi」だと思っています。この方が覚えやすい。

<pre><code># dnssec-keygen -r /dev/urandom -a RSASHA256 -b 1024 aimless.jp &gt; zsk-aimless.jp
# dnssec-keygen -r /dev/urandom -a RSASHA256 -b 1024 -f ksk aimless.jp &gt; ksk-aimless.jp
</code></pre>

　KSKのビット長を間違えました。。。実行結果をZSK/KSKの名称の付いたファイルにリダイレクトしているのは、鍵の用途を分かりやすくするためです。作成されたZSK鍵とKSK鍵鍵はファイル名にZSK/KSKの文字が記載されないので、一目見ただけではZSKかKSKを識別出来ません。識別するには、ファイルの中を確認しDNSKEYの次が256か257かを確認するしかありません。

<pre><code># more Kaimless.zone.+005+23820.key 
aimless.zone. IN DNSKEY 256 3 5 AwEA・・・・
</code></pre>

　今後の作業で鍵名を使ってコマンドを実行するにも関わらず、ファイルの用途がファイル名で分からないのはイマイチです。コマンドを打つ前に毎回moreでZSKかKSKかを確認するのは面度です。そこで、鍵を生成する際に、鍵のファイル名を鍵の種類が分かるファイルにリダイレクトしておきます。上記コマンドで作成されたファイルの中身は下記の通り。

<pre><code># more a zsk-aimless.jp 
Kaimless.jp.+008+28518</code></pre>

　コマンド内で鍵のファイル名を指定する部分を、このファイルをcatで開くコマンド＋.keyに置換する事で、これはどっちの鍵？問題を解決します。JPRSの資料に書いてあるのですが、この方法を利用すると鍵の取り扱いがかなり楽になります。これはスゴイ。

### ゾーンファイルに公開鍵を追記

　作成したZSK公開鍵とKSK公開鍵を、INCLUDEを使いゾーンファイルに追記します。

<pre><code>$TTL    3600
$INCLUDE Kaimless.jp.+008+30799.key
$INCLUDE Kaimless.jp.+008+28518.key
</code></pre>

### ゾーンファイルへの署名

　作成した秘密鍵を使い、ゾーンファイルに署名します。ここで判明したのですが、秘密鍵が同じディレクトリに存在すれば、鍵を指定しなくてもいいらしいです。鍵のファイル名をリダイレクトした意味なし。。。

<pre><code># dnssec-signzone -o aimless.jp -N unixtime aimlesss.zone
Verifying the zone using the following algorithms: RSASHA256.
Zone signing complete:
Algorithm: RSASHA256: KSKs: 1 active, 0 stand-by, 0 revoked
                      ZSKs: 1 active, 0 stand-by, 0 revoked
aimless.zone.signed
</code></pre>

　無事成功すると、署名済みのゾーンファイル（aimless.zone.signed）とDSレコードが記載されたファイル（dsset-aimless.jp.）が作成されます。署名済みのファイルはDNSKEYレコードとRRSIGレコード、NSECレコードが追記されカオスになりました。。。ファイルサイズは署名前と比べて10倍です。

### named.confの設定変更

　bindが利用するゾーンファイルを署名済み物に変更します。また、dnssecを有効化する一文を追加します。変更後にbindを再起動します。

<pre><code>zone "aimless.jp" {
                type master;
                file "aimless.zone.signed";

        dnssec-enable yes;
</code></pre>

### 上位DNSへのDSレコード追加

　信頼の連鎖を構築すべく、自ドメインの指定事業者である21-domain.comに、JPRSのa.dns.jpへのDSレコード登録を依頼します。21:45頃、担当の方にメールでdsset-aimless.jpを送付した所、30分後に対応して頂けました。この指定事業者は、IPv6とDNSSSEC両方に対応しているデキル子です。

### 動作確認

　Verisignが提供しているDNSSECの動作確認サービス [（DNSSEC Debugge）][1]を使い、信頼の連鎖が構築されているかどうかを確認します。

![dnssec-result][2]

　全て緑ですので、rootから自ドメインまで信頼の連鎖が繋がったようです。いずれは、DNSSECのキモである鍵の更新に挑戦しようと思います。

 [1]: http://dnssec-debugger.verisignlabs.com/
 [2]: http://aimless.jp/images/dnssec_result.png