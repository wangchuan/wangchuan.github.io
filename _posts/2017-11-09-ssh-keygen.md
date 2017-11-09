---
layout: post
title:  "SSH Keygen"
categories: coding
tags: linux
---

* TOC
{:toc}

### Generate the key in your own computer
```bash
ssh-keygen
```
Then you only need to press <key>Enter</key> so that the key is generated and saved in `~/.ssh/id_rsa`.

### Put it into the server
#### Copy the `id_rsa.pub` into your server:
```bash
scp ~/.ssh/id_rsa.pub username@10.127.1.155:~/.ssh/
```
#### Add it into the `authorized_keys`
```bash
cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys
```
