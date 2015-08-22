---
title: Vyattaで経路制御②(local preferenceで経路選択)
author: kongou_ae
layout: post
date: 2011-07-05
url: /blog/archives/236
categories:
  - IHANet
  - Vyatta
---
「いずれIHANetにもIHANet内のフルルートを提供するトランジットが出るはずだ！」という事で、トランジットから広告された経路ではなく、Peerから直接広告してもらっている経路を優先する設定を事前にしておきたいと思います。

インターネットの経済学みたいな話になりますが、トランジットはフルルートを貰えるだけあって利用料金が高い。そこで下図の様に、InternetExchange経由で別ASと直接Peeringを行い、経路情報をやりとりします。そして、トランジットとPeerから同じ経路を受け取った場合に、Peerの経路を優先する様に設定します。

![bgp][1]

InternetExchange経由でトラフィックをやりとりするのにも当然費用がかかりますが、大概トランジットを利用するよりも安くなります。こうする事でデータセンタの通信コストを下げます。データセンタ事業者からすれば、大量の通信が発生するコンテンツプロバイダとは、是非IX経由でpeeringしたいところです。

### 作業前の経路情報

設定前にpeerから受け取った経路2001:\*\*:\*\*:2a20::/64のLocPrfは空欄です。IPv6だと表示が崩れてしまうのが残念。

<pre><code>$ show ipv6 bgp
BGP table version is 0, local router ID is 49.212.54.72
Status codes: s suppressed, d damped, h history, * valid, &gt; best, i - internal,
              r RIB-failure, S Stale, R Removed
Origin codes: i - IGP, e - EGP, ? - incomplete

   Network          Next Hop            Metric LocPrf Weight Path
*&gt; 2001:**:**:2a20::/64
                    2001:**:**:fc::6
                                                           0 645** 645** 645** ?
</code></pre>

### local preference値を付与するroute-mapを作成する

Peerからの経路は全てLocal preference値を付与するので、matchによるprefixの指定は行いません。LPというroute-mapを作成し、local-preference値400を付与します。line5-6の部分です。

<pre><code>route-map LP {
     description set-Local-Preference
     rule 100 {
         action permit
         set {
             local-preference 400
         }
     }
 }
</code></pre>

### route-mapをpeerと関連付ける

peerとの関連付けは、その①で作成したpeer-groupで行います。peer-group IHAnet に先程作成したroute-mapを関連付けます。line19-20の部分です。

<pre><code>protocols {
    bgp 64585 {
        address-family {
            ipv6-unicast {
                network 2001:e41:31d4:3648::/64 {
                }
            }
        }
        peer-group IHAnet {
            address-family {
                ipv6-unicast {
                    filter-list {
                        import peer-path
                    }
                    prefix-list {
                        export this-network
                        import Special-Use-prefix
                    }
                    route-map {
                        import LP
                    }
                    soft-reconfiguration {
                        inbound
                    }
</code></pre>

### 設定後の確認

peerから受け取った経路2001:\*\*:\*\*:2a20::/64のLocPrfに400が付与されています。この状態でトランジットから同じ経路を広告されたとしても、トランジット側のLocal Preference値は何もしていない＝デフォルトの100なので、Peer側の経路がベストパスとして選択されます。

<pre><code># show ipv6 bgp
BGP table version is 0, local router ID is 49.212.54.72
Status codes: s suppressed, d damped, h history, * valid, &gt; best, i - internal,
              r RIB-failure, S Stale, R Removed
Origin codes: i - IGP, e - EGP, ? - incomplete

   Network          Next Hop            Metric LocPrf Weight Path
*&gt; 2001:**:**:2a20::/64
                    2001:**:**:fc::6
                                                  400      0 645** 645** 645** ?
</code></pre>

 [1]: http://aimless.jp/images/bgp.png