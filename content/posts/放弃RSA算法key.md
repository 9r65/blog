---
title: "放弃RSA算法key"
date: 2023-09-27
draft: false
tags: ["linux","加密","备忘"]

---

今天再配置一台新服务器的时候, 无意间看见一篇blog在讲述"初始化配置linux服务器"时提到了:

> 配置通过ssh key 登录服务器 , 本地应该生成**ssh-ed25519**算法的key , 而放弃**ssh-rsa**公钥签名算法生成的key


经过搜索后, 发现应该是在**OpenSSH 8.3**版本发布后的一则声明 :
> 
> Future deprecation notice
> =========================
> 
> It is now possible[1] to perform chosen-prefix attacks against the
> SHA-1 algorithm for less than USD$50K. For this reason, we will be
> disabling the "ssh-rsa" public key signature algorithm by default in a
> near-future release.
> 
> This algorithm is unfortunately still used widely despite the
> existence of better alternatives, being the only remaining public key
> signature algorithm specified by the original SSH RFCs.

这里是找到的详细信息 : [OpenSSH 8.3 released](https://lwn.net/Articles/821544/)

文章里提到了替代方案:


>The better alternatives include:
>
> * The RFC8332 RSA SHA-2 signature algorithms rsa-sha2-256/512. These
>   algorithms have the advantage of using the same key type as
>   "ssh-rsa" but use the safe SHA-2 hash algorithms. These have been
>   supported since OpenSSH 7.2 and are already used by default if the
>   client and server support them.
>
> * The ssh-ed25519 signature algorithm. It has been supported in
>   OpenSSH since release 6.5.
>
> * The RFC5656 ECDSA algorithms: ecdsa-sha2-nistp256/384/521. These
>   have been supported by OpenSSH since release 5.7.


目前打算开始逐步更换之前生成RSA的key , 直接使用 **ssh-ed25519**签名算法 生成新的key , 同时github的帮助文档里 , 在[生成新的ssh key](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent#generating-a-new-ssh-key)中也推荐了使用**ssh-ed25519**签名算法来生成


附上生成命令:
```json

ssh-keygen -t ed25519 -C "your_email@example.com"

```
