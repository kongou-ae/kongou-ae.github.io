---
title: CiscoのBGP4-MIBはIPv6未対応？？
author: kongou_ae
layout: post
date: 2012-03-25
url: /blog/archives/738
categories:
  - IHANet
  - network
---
　「HEのBGP Toolkitで見れるAS接続図を自分のASでもやってみよう」と思いgrahpvizで作ってみました。<a href="http://dns3.aimless.jp/asgraph.gif" title="AS64585接続図" target="_blank">AS64585接続図</a>

　元となるAS-PATHの情報は「show bgp ipv6 unicast paths」から引っ張ってきました。ルータの出力をいちいちコピペ&手作業で整形するのが面倒なので、SMNPで取れた情報を自動成型できたら素敵だなとsnmpwalkで探してみたのですが、肝心のAS-Path関連はnot-accessibleだったので諦めました。

　そんなことより、IHANetのIPv6ネイバーがSNMPで取得できなかったのですが、CiscoのBGP4-MIBはIPv6未対応なのでしょうか。

### IPv6のネイバしかいない場合

　bgpPeerTable（1.3.6.1.2.1.15.3.1）の中身が存在しません。

<pre><code># snmpwalk -v 2c -c public xxx.aimless.jp .1.3.6.1.2.1.15
SNMPv2-SMI::mib-2.15.1.0 = Hex-STRING: 10 
SNMPv2-SMI::mib-2.15.2.0 = INTEGER: 64585
SNMPv2-SMI::mib-2.15.4.0 = IpAddress: 36.2.xxx.xxx
</code></pre>

### IPv4のネイバがいる場合

　bgpPeerTable（1.3.6.1.2.1.15.3.1）の中身が存在します。ただしIPv4だけ。。。

<pre><code># snmpwalk -v 2c -c public xxx.aimless.jp .1.3.6.1.2.1.15
SNMPv2-SMI::mib-2.15.1.0 = Hex-STRING: 10 
SNMPv2-SMI::mib-2.15.2.0 = INTEGER: 64585
SNMPv2-SMI::mib-2.15.3.1.1.10.10.10.10 = IpAddress: 0.0.0.0
SNMPv2-SMI::mib-2.15.3.1.2.10.10.10.10 = INTEGER: 1
SNMPv2-SMI::mib-2.15.3.1.3.10.10.10.10 = INTEGER: 2
SNMPv2-SMI::mib-2.15.3.1.4.10.10.10.10 = INTEGER: 4
SNMPv2-SMI::mib-2.15.3.1.5.10.10.10.10 = IpAddress: 0.0.0.0
SNMPv2-SMI::mib-2.15.3.1.6.10.10.10.10 = INTEGER: 0
SNMPv2-SMI::mib-2.15.3.1.7.10.10.10.10 = IpAddress: 10.10.10.10
SNMPv2-SMI::mib-2.15.3.1.8.10.10.10.10 = INTEGER: 0
SNMPv2-SMI::mib-2.15.3.1.9.10.10.10.10 = INTEGER: 1111
SNMPv2-SMI::mib-2.15.3.1.10.10.10.10.10 = Counter32: 0
SNMPv2-SMI::mib-2.15.3.1.11.10.10.10.10 = Counter32: 0
SNMPv2-SMI::mib-2.15.3.1.12.10.10.10.10 = Counter32: 0
SNMPv2-SMI::mib-2.15.3.1.13.10.10.10.10 = Counter32: 0
SNMPv2-SMI::mib-2.15.3.1.14.10.10.10.10 = Hex-STRING: 00 00 
SNMPv2-SMI::mib-2.15.3.1.15.10.10.10.10 = Counter32: 0
SNMPv2-SMI::mib-2.15.3.1.16.10.10.10.10 = Gauge32: 0
SNMPv2-SMI::mib-2.15.3.1.17.10.10.10.10 = INTEGER: 60
SNMPv2-SMI::mib-2.15.3.1.18.10.10.10.10 = INTEGER: 0
SNMPv2-SMI::mib-2.15.3.1.19.10.10.10.10 = INTEGER: 0
SNMPv2-SMI::mib-2.15.3.1.20.10.10.10.10 = INTEGER: 180
SNMPv2-SMI::mib-2.15.3.1.21.10.10.10.10 = INTEGER: 60
SNMPv2-SMI::mib-2.15.3.1.22.10.10.10.10 = INTEGER: 30
SNMPv2-SMI::mib-2.15.3.1.23.10.10.10.10 = INTEGER: 30
SNMPv2-SMI::mib-2.15.3.1.24.10.10.10.10 = Gauge32: 0
SNMPv2-SMI::mib-2.15.4.0 = IpAddress: 36.2.xxx.xxx
</code></pre>