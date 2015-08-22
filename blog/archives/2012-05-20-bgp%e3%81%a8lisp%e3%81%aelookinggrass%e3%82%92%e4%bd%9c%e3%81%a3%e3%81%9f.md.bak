---
title: BGPとLISPのLookingGrassを作った
author: kongou_ae
layout: post
date: 2012-05-19
url: /blog/archives/1039
categories:
  - IHANet
  - network
---
</p> 

　ruby on rails + apache2 + passengerで、AS64585のLookingGrassを作りました。<a href="http://lg.aimless.jp/" title="LookingGrass（AS64585）" target="_blank">LookingGrass（AS64585）</a>

　当初は他ASさんが運営しているLookingGrassと同様、インターネット上に全公開する予定でした。しかし、neighborさんのIPアドレスを全公開するのもあれだなと思い、eBGPルータが受け取っている経路とLISP-Betaで利用されている経路でアクセス制御を掛けました。現在106経路ほどBGPテーブルに乗っているので、IHANet上の方々であればみられるのかなと思います。

　なお、BGPだけだと面白くないので、LISPについても見れるようにしてあります。「LISPを利用しているIPv6の通信がそれほどない」＆「基本的にはすべてのマップキャッシュはPETR向け」という事で、それほど面白くないのが残念ですが・・・