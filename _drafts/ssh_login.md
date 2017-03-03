---
layout: post
title: ssh登录错误及解决
category: linux ssh login
tags: [ssh, Permission denied, 权限, authorized_keys]
---


## 问题描述sd


www.delehi.net的delehi用户无法用ssh登录
显示如下错误
```bash
Permission denied (publickey,gssapi-keyex,gssapi-with-mic).
```
完整显示如下：
```bash
ssh -2 -v delehi@www.delehi.net -p 40022 -i id_rsa_delehi.ppk [11:48:05]
OpenSSH_7.3p1, LibreSSL 2.4.1
debug1: Reading configuration data /Users/aron/.ssh/config
debug1: Reading configuration data /etc/ssh/ssh_config
debug1: /etc/ssh/ssh_config line 20: Applying options for *
debug1: /etc/ssh/ssh_config line 56: Applying options for *
debug1: Connecting to www.delehi.net [125.206.209.35] port 40022.
debug1: Connection established.
debug1: key_load_public: No such file or directory
debug1: identity file id_rsa_delehi.ppk type -1
debug1: key_load_public: No such file or directory
debug1: identity file id_rsa_delehi.ppk-cert type -1
debug1: Enabling compatibility mode for protocol 2.0
debug1: Local version string SSH-2.0-OpenSSH_7.3
debug1: Remote protocol version 2.0, remote software version OpenSSH_5.3
debug1: match: OpenSSH_5.3 pat OpenSSH_5* compat 0x0c000000
debug1: Authenticating to www.delehi.net:40022 as 'delehi'
debug1: SSH2_MSG_KEXINIT sent
debug1: SSH2_MSG_KEXINIT received
debug1: kex: algorithm: diffie-hellman-group-exchange-sha256
debug1: kex: host key algorithm: ssh-rsa
debug1: kex: server->client cipher: aes128-ctr MAC: umac-64@openssh.com compression: none
debug1: kex: client->server cipher: aes128-ctr MAC: umac-64@openssh.com compression: none
debug1: SSH2_MSG_KEX_DH_GEX_REQUEST(2048<3072<8192) sent
debug1: got SSH2_MSG_KEX_DH_GEX_GROUP
debug1: SSH2_MSG_KEX_DH_GEX_INIT sent
debug1: got SSH2_MSG_KEX_DH_GEX_REPLY
debug1: Server host key: ssh-rsa SHA256:J3D899zZ1h1LPH4N6vbE5LPxhGvASZ0tN71+DyAJgYo
debug1: Host '[www.delehi.net]:40022' is known and matches the RSA host key.
debug1: Found key in /Users/aron/.ssh/known_hosts:5
debug1: rekey after 4294967296 blocks
debug1: SSH2_MSG_NEWKEYS sent
debug1: expecting SSH2_MSG_NEWKEYS
debug1: rekey after 4294967296 blocks
debug1: SSH2_MSG_NEWKEYS received
debug1: SSH2_MSG_SERVICE_ACCEPT received
debug1: Authentications that can continue: publickey,gssapi-keyex,gssapi-with-mic
debug1: Next authentication method: publickey
debug1: Trying private key: id_rsa_delehi.ppk
Enter passphrase for key 'id_rsa_delehi.ppk':
debug1: No more authentication methods to try.
Permission denied (publickey,gssapi-keyex,gssapi-with-mic).
```
经过多次试验发现问题出在服务器端的.ssh/authorized_keys的权限
```plain
-rw-r--r-- 1 delehi delehi  403  1月 20 12:34 2017 authorized_keys
```
经过下面的修改后，问题解决, 可以登录
```plain
chmod 644 authorized_keys

-rw------- 1 delehi delehi  403  1月 20 12:34 2017 authorized_keys
```

## 遗留的问题

但是，在修改成原来的权限以后，也能登录，这就不知道为什么了
```lasso
[delehi@ws1 .ssh]$ chmod 644 authorized_keys
[delehi@ws1 .ssh]$ ll
合計 8
-rw-r--r-- 1 delehi delehi  403  1月 20 12:34 2017 authorized_keys
```