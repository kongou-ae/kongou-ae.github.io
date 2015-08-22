---
title: BGP Multipath
author: kongou_ae
layout: post
date: 2012-09-23
url: /blog/archives/1326
categories:
  - network
---
　BGP Multipathの動作確認をしました。構成は下記の通りです。トランジットと2本の専用線で接続し、BGPを2セッション貼るイメージです。

![multipath][1]

### 通常の場合

<pre><code>### configuration of Left ###
router bgp 65001
 bgp router-id 10.0.0.1
 bgp log-neighbor-changes
 network 192.168.1.0
 network 192.168.2.0
 neighbor 10.0.1.254 remote-as 65002
 neighbor 10.0.2.254 remote-as 65002
!
ip route 192.168.1.0 255.255.255.0 Null0
ip route 192.168.2.0 255.255.255.0 Null0

### configuration of Right###
router bgp 65002
 bgp router-id 10.0.0.254
 bgp log-neighbor-changes
 neighbor 10.0.1.1 remote-as 65001
 neighbor 10.0.2.1 remote-as 65001
 !
 address-family ipv4
  neighbor 10.0.1.1 activate
  neighbor 10.0.2.1 activate
 exit-address-family
!
</code></pre>

　Rightは各セッションでLeftが広告した2経路を受信していますが、ルーティングテーブルにはベストパスを1つしか乗せません。おそらくベストパス選択アルゴリズムの最後「最小の隣接ルータ アドレスから送られたパスが優先されます。」で選択したものだと思います。

　ルーティングテーブルにはベストパス1つしか乗っていないので、専用線を2本引いているにも関わらず、片方しか使われません。冗長自体は別のASと接続する事で実現するので、単一ASとの接続は全ての専用線を使ってくれた方がうれしい場合もあります。そんな時にMultipathを使うのかな。

<pre><code>right#show bgp ipv4 unicast summary 

Neighbor        V           AS MsgRcvd MsgSent   TblVer  InQ OutQ Up/Down  State/PfxRcd
10.0.1.1        4        65001      12      12        5    0    0 00:07:33        2
10.0.2.1        4        65001      12      12        5    0    0 00:07:33        2

right#show ip route

B     192.168.1.0/24 [20/0] via 10.0.1.1, 00:01:46
B     192.168.2.0/24 [20/0] via 10.0.1.1, 00:01:46
</code></pre>

### Multipathを有効にした場合

<pre><code>### configuration of Right ###
router bgp 65002
 bgp router-id 10.0.0.254
 bgp log-neighbor-changes
 neighbor 10.0.1.1 remote-as 65001
 neighbor 10.0.2.1 remote-as 65001
 !
 address-family ipv4
  neighbor 10.0.1.1 activate
  neighbor 10.0.2.1 activate
  maximum-paths 2 ←追加!!!
 exit-address-family
</code></pre>

　各セッションでLeftが広告した2経路を受信しており、さらにルーティングテーブルにはベストパスが2つ乗っています。ルーティングテーブルに2経路乗ればECMPで分散されますので、2本の専用線が両方とも利用されます。通常の構成と比較して、線の本数は変わらないのにトランジットへの接続帯域を倍にできるわけですね。

<pre><code>right#show bgp ipv4 unicast summary 

Neighbor        V           AS MsgRcvd MsgSent   TblVer  InQ OutQ Up/Down  State/PfxRcd
10.0.1.1        4        65001      16      17        7    0    0 00:11:53        2
10.0.2.1        4        65001      17      16        7    0    0 00:11:53        2

right#show ip route

B     192.168.1.0/24 [20/0] via 10.0.2.1, 00:03:19
                     [20/0] via 10.0.1.1, 00:03:19
B     192.168.2.0/24 [20/0] via 10.0.2.1, 00:03:19
                     [20/0] via 10.0.1.1, 00:03:19
</code></pre>

 [1]: http://aimless.jp/images/multipath.png