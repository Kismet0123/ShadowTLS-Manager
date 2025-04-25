# ShadowTLS-Manager

[![Top Language](https://img.shields.io/github/languages/top/Kismet0123/ShadowTLS-Manager.svg)](https://github.com/Kismet0123/ShadowTLS-Manager) [![GitHub Stars](https://img.shields.io/github/stars/Kismet0123/ShadowTLS-Manager.svg?style=social)](https://github.com/Kismet0123/ShadowTLS-Manager) [![GitHub Forks](https://img.shields.io/github/forks/Kismet0123/ShadowTLS-Manager.svg?style=social)](https://github.com/Kismet0123/ShadowTLS-Manager)

ShadowTLS 管理脚本，支持一键安装、升级和卸载，默认使用v3(strict)版本（能够防御流量特征检测、主动探测和流量劫持）

> [!IMPORTANT]
> ShadowTLS 不包含数据加密和代理请求封装功能，需配合SS、Snell、Trojan等协议使用

**推荐使用SS2022+ShadowTLS**，相比于单独使用SS2022，ShadowTLS在其基础上进行了真实的TLS握手，并且使用 Application Data 封装与解封装数据。在此情况下，中间人观测该流量与正常的https流量特征一致  
注：在执行安装时脚本会自动读取ss-rust、xray和singbox配置文件中shadowsocks的配置，以自动识别后端服务端口，并且支持输出Surge、Loon、Shadowrocket、Mihomo和Singbox的配置

---

## 快速开始📃

**一键部署命令**

```bash
wget -O ShadowTLS_Manager.sh --no-check-certificate https://raw.githubusercontent.com/Kismet0123/ShadowTLS-Manager/refs/heads/main/ShadowTLS_Manager.sh && chmod +x ShadowTLS_Manager.sh && ./ShadowTLS_Manager.sh
```

**查看日志**

```
journalctl -f -o cat -n 100 -u shadow-tls
```

**删除脚本**

```
rm ShadowTLS_Manager.sh
```


# SS2022+ShadowTLS-v3部署教程(适合新手)⭐️

### 一、使用ss-rust脚本部署ss2022

1. 执行下方一键代码

```bash
wget -O ss-rust.sh --no-check-certificate https://raw.githubusercontent.com/xOS/Shadowsocks-Rust/master/ss-rust.sh && chmod +x ss-rust.sh && ./ss-rust.sh
```

2. 按数字1后回车，输入ss端口，默认为2525
3. 加密方式选择13.2022-blake3-aes-128-gcm即可，回车随机生成密码，TCP Fast Open建议关闭
4. 记录ss2022配置信息

### 二、部署ShadowTLS

1. 执行下方一键代码

```bash
wget -O ShadowTLS_Manager.sh --no-check-certificate https://raw.githubusercontent.com/Kismet0123/ShadowTLS-Manager/refs/heads/main/ShadowTLS_Manager.sh && chmod +x ShadowTLS_Manager.sh && ./ShadowTLS_Manager.sh
```

2. 按数字1后回车，输入上面部署的ss2022端口，在此以2525为例 (脚本可以自动识别已安装的ss端口，确认无误可以直接回车)
3. 输入TLS伪装域名，需支持TLS1.3
4. 输入密码，直接回车则自动生成
5. 输入ShadowTLS外部监听端口，即最终使用的端口，默认为443
6. 按需开启泛域名SNI和fastopen
7. 安装完毕后回到主菜单按数字键4查看配置信息

### 三、注意关闭系统防火墙或放行相应端口

如果需要启用防火墙，脚本能够自动放行相应端口  
例如CentOS、Almalinux自带的firewalld，关闭firewalld命令（一行一行执行）

```
systemctl stop firewalld
systemctl disable firewalld
```

---

## **注意不要混淆多个端口⚠️**

**后端服务端口**（2525）是ss2022的端口，使用客户端如Loon、Surge等配置时要填写**外部监听端口**（443），如需配置udp端口则填入后端服务端口（2525）  
后端服务端口并无特殊要求，外部监听端口尽可能使用443，以达到最佳伪装效果  
注：443 和 2525 为脚本默认端口

## 客户端配置参考🖥️

### Loon 配置参考示例（192.168.1.1为服务器ip）

```
SS2022+sTLS = Shadowsocks,192.168.1.1,443,2022-blake3-aes-128-gcm,"ss2022密码",shadow-tls-password=shadowtls密码,shadow-tls-sni=www.tesla.com,shadow-tls-version=3,udp-port=2525,ip-mode=v4-only,fast-open=false,udp=true
```

### Surge 配置参考示例（192.168.1.1为服务器ip）

```
SS2022+sTLS = ss, 192.168.1.1, 443, encrypt-method=2022-blake3-aes-128-gcm, password="ss2022密码", ip-version=v4-only, shadow-tls-password="shadowtls密码", shadow-tls-sni=www.tesla.com, shadow-tls-version=3, udp-relay=true, udp-port=2525
```

### Mihomo 配置参考示例（192.168.1.1为服务器ip）

```
- {"name":"SS2022+sTLS","type":"ss","server":"192.168.1.1","port":443,"cipher":"2022-blake3-aes-128-gcm","password":"ss2022密码","udp":true,"udp-over-tcp":true,"udp-over-tcp-version":"2","ip-version":"ipv4","plugin":"shadow-tls","client-fingerprint":"chrome","plugin-opts":{"host":"www.tesla.com","password":"shadowtls密码","version":3}}
```

## 支持ShadowTLS-v3的客户端

| 🎈 Loon      | [nsloon.app](https://nsloon.app/)                       |
|:------------:|:-------------------------------------------------------:|
| 🌊 Surge     | [nssurge.com](https://nssurge.com/)                    |
| ☘️ Stash    | [stash.ws](https://stash.ws/)                         |
| 🐿️ Egern   | [egernapp.com](https://egernapp.com/)                |
| 🚀 Shadowrocket | N/A                                            |
| 🐱 Mihomo   | [wiki.metacubex.one](https://wiki.metacubex.one/config/proxies/ss/) |
| 🎁 Singbox | [sing-box.sagernet.org](https://sing-box.sagernet.org/zh/configuration/inbound/shadowtls/) |

---

# 参考资料📚

## [ShadowTLS原仓库](https://github.com/ihciah/shadow-tls)

[ShadowTLS的设计细节-由ihciah大佬撰写（推荐阅读）](https://www.ihcblog.com/a-better-tls-obfs-proxy/)

## ShadowTLS伪装域名选择

参考[原仓库Wiki](https://github.com/ihciah/shadow-tls/wiki/V3-Protocol)

## ShadowTLS CPU占用率高解决方法

脚本中已提供了修复选项  
参考[原仓库issues](https://github.com/ihciah/shadow-tls/issues/109)

## 推荐项目

[SS2022一键部署-翠花](https://github.com/xOS/Shadowsocks-Rust)

[Snell一键部署-翠花](https://github.com/xOS/Snell)

[Snell一键部署-jinqians](https://github.com/jinqians/snell.sh)

# Star增长趋势

![Star Trend](https://starchart.cc/Kismet0123/ShadowTLS-Manager.svg )
