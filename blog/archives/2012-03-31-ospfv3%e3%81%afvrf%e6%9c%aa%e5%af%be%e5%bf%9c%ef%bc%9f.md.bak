---
title: OSPFv3はVRF未対応？
author: kongou_ae
layout: post
date: 2012-03-31
url: /blog/archives/748
categories:
  - cisco
  - network
---
　タイトルのままです。環境はCisco892J：Version 15.1(4)XB7です。vrfが設定されているインターフェースでOSPFv3を動かそうとすると、サポートされてないよ旨のエラーメッセージが出ました。

　15.2Tのリリースノートを眺めて見てもそれっぽい記載はないので、最新のIOSでもサポートされていないのかもしれません。

　VRFによる自宅NWのリプレースを予定していたのですが、自AS内の経路交換にOSPFv3を使っているので断念しました。。。残念

<pre><code>interface Tunnel1
 description SakuraVPS
 vrf forwarding ihanet
 no ip address
 ipv6 mtu 1280
 tunnel source 36.2.xxx.xxx
 tunnel destination 49.212.xxx.xxx
end
Router(config-if)# ipv6 address 2001:E41:31D4:3648::xxx/126
Router(config-if)# ipv6 enable
Router(config-if)# ipv6 mtu 1280
Router(config-if)# ipv6 ospf 1 area 0.0.0.0 instance 100
% OSPFv3: not supported on VRF interface
</code></pre>