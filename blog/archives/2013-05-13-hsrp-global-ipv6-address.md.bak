---
title: 'HSRP: Global IPv6 Address'
author: kongou_ae
layout: post
date: 2013-05-13
url: /blog/archives/1607
categories:
  - cisco
  - network
---
　15.3(2)TからHSRPのVIPとしてGUAがサポートされたようなので試してみました。

### CISCO892-K9 IOS 15.3(1)Tの場合

    Router(config)#interface vlan 15 
    Router(config-if)#standby ? 
    Router(config-if)#standby ipv6 ? 
    X:X:X:X::X IPv6 link-local address autoconfig Obtain address using autoconfiguration
    

### CISCO892-K9 IOS 15.3(2)Tの場合

    Router(config)#interface vlan 15 
    Router(config-if)#standby 0 ipv6 ? 
    X:X:X:X::X IPv6 link-local address 
    X:X:X:X::X/<0-128> IPv6 prefix autoconfig Obtain address using autoconfiguration 
    Router(config-if)#standby 0 ipv6 2001:db8::1/64 
    Router(config-if)#end 
    Router#show standby brief 
    all Load for five secs: 49%/0%; one minute: 24%; five minutes: 10% Time source is NTP, 23:23:14.015 JST Mon May 13 2013 
    P indicates configured to preempt.
    | 
    Interface Grp Pri P State Active Standby Virtual IP 
    Vl15 0 100 Init unknown unknown FE80::5:73FF:FEA0:0 (impl auto EUI64) 
    Vl15 0 100 Init unknown unknown 2001:DB8::1/64 
    Router#