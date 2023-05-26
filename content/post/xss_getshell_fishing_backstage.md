---
title: "记一次拿钓鱼网站后台的过程"
date: 2022-05-08T22:53:16+08:00
categories: ["notes"]
---

早上醒来，发现同学晚上给我来了个QQ电话，同时自己还被对方删除好友了，这一反常的现象，我立马察觉不对。再看了看QQ空间，发现同学发动态说自己号被盗了，于是我重新加了同学的好友，确认了对方身份后，我问了问同学被盗的号的过程，同学发来了一个腾讯文档，题目是《2022年教务管理系统学生学籍电子档案核对信息通知》，打开腾讯文档里面是一个二维码。我这一看，这不就是一个骗信息的嘛，最近老看到这种消息，没想到自己身边的同学也中了招。我顿时有点不服，这拙劣的诈骗手段居然把我同学骗了，于是想拿下后台帮同学把盗取的信息删除。

我首先用二维码网站识别工具，识别出了其中的网站，用浏览器打开，发现这就是个活脱脱的钓鱼网站

![dy01](https://s2.loli.net/2022/05/08/YjyF37JLXfQzBDk.png)
这个你随便输入什么都可以登录，登录了就是叫你填身份证、电话等信息
![dy02](https://s2.loli.net/2022/05/08/eAFYZOKMPyXdtVl.png)
这种钓鱼网站，前两天看到有师傅发过相关的文章

渗透的思路：

1. 先找到网站的IP，这种网站一般都不会有CDN，所以一个超级ping直接找到IP

2. 根据域名查一下whois信息，查出来一个QQ号，不知道是不是真的，QQ号的等级不高也不低

3. 做网站路径扫描，找管理后台，结果没扫出来

4. 根据网站的功能点找思路，这个网站只有一个登录框、输入表单，功能点很少，此时后台也找不到，所以尝试XSS盲打，觉得用XSS盲打就算最后盗不了Cookie，起码也能找个管理后台地址，再尝试SQL注入登录后台也可以呀。

5. 随便在网上找了一个XSS平台开搞，把XSS payload插入到登录框、输入表单当中，反正就是见框就插，最后终于在两天后发现XSS平台获取到了相关信息。

   ![dy03](https://s2.loli.net/2022/05/08/EbMaqYyJtBvk2G7.png)
   幸运的是，不仅有后台管理地址，还有cookie，直接使用cookie登录后台
   ![dy04](https://s2.loli.net/2022/05/08/9wcgSVm3INT5JnD.png)
   登录后，找了好多被盗取的QQ号和密码、身份证等敏感信息
   ![dy05](https://s2.loli.net/2022/05/08/KE3uRc1FDIHqfh9.png)
   进入后台后，有尝试进行getshell，但是这个后台功能点太少，而且对php文件做了限制，最终没能getshell（菜是原罪)。

   本想帮我同学把盗取的信息删除的，但是我这时在后台找不到了，我想骗子可能把信息进行转存了，也许拿了shell应该能有收获吧!

   这次对钓鱼网站拿后台的过程，并没有什么技术含量，就是一个烂大街的XSS盲打。