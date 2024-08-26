# AdGuard Home upstream_dns_file

基于域名的简单分流 不完美 按需使用

## 社群

👏🏻 欢迎加入社群进行交流讨论

👥 群组 [折腾啥(群组)](https://t.me/zhetengsha_group)

📢 频道 [折腾啥(频道)](https://t.me/zhetengsha)

## 使用方法

### `china.txt` 国内规则使用国内 DNS, 其他使用国外 DNS

下载(releases): [https://github.com/xream/AdGuardHomeUpstreamDnsFile/releases/latest/download/china.txt](https://github.com/xream/AdGuardHomeUpstreamDnsFile/releases/latest/download/china.txt)

下载(branch): [https://raw.githubusercontent.com/xream/AdGuardHomeUpstreamDnsFile/release/china.txt](https://raw.githubusercontent.com/xream/AdGuardHomeUpstreamDnsFile/release/china.txt)

数据来源: [blackmatrix7/ios_rule_script ChinaMaxNoIP](https://github.com/blackmatrix7/ios_rule_script/tree/master/rule/Surge/ChinaMaxNoIP)

格式:

```
# > 来自 DOMAIN-SUFFIX,a.com
# > 表示 a.com, *.a.com 全部分流到 __CHINA_DNS_PLACEHOLDER__
[/a.com/b.com/]__CHINA_DNS_PLACEHOLDER__

# > 来自 DOMAIN,b.com
# > 表示 仅 b.com 分流到 __CHINA_DNS_PLACEHOLDER__
# > 在上面生成一条 b.com 分流到 __CHINA_DNS_PLACEHOLDER__
# > 在这里生成一条 *.b.com 分流到 __GLOBAL_DNS_PLACEHOLDER__
[/*.b.com/]__GLOBAL_DNS_PLACEHOLDER__
```

因为数据源是代理 App 的分流规则, AdGuard Home 基于域名的简单分流, 就这样吧...

`__CHINA_DNS_PLACEHOLDER__` 和 `__GLOBAL_DNS_PLACEHOLDER__` 自行替换即可

在实际使用中, 可能应当将本文件的两者都替换成 `国内 DNS`

一个例子, 仅供参考, 可自行修改, 设为定时任务:

```bash
#!/usr/bin/env bash
# > 下载文件到 /opt/adguardhome/conf/china.txt
curl -L https://github.com/xream/AdGuardHomeUpstreamDnsFile/releases/latest/download/china.txt -o /opt/adguardhome/conf/china.txt
# > 修改文件内容
# > 分流到 国内 DNS
sed -i "s|__CHINA_DNS_PLACEHOLDER__|https://doh-pure.onedns.net/dns-query https://120.53.53.53/dns-query https://223.6.6.6/dns-query|g" /opt/adguardhome/conf/china.txt
sed -i "s|__GLOBAL_DNS_PLACEHOLDER__|https://doh-pure.onedns.net/dns-query https://120.53.53.53/dns-query https://223.6.6.6/dns-query|g" /opt/adguardhome/conf/china.txt
# > 其他分流到 国外 DNS
sed -i "1s|^|tls://9.9.9.11\ntls://208.67.220.220\ntls://8.8.4.4\n|" /opt/adguardhome/conf/china.txt
```

修改 `/opt/adguardhome/conf/AdGuardHome.yaml` 中的 `upstream_dns_file` 为 `/opt/adguardhome/conf/china.txt`

重启 AdGuard Home

### `global.txt` 国外规则使用国外 DNS, 其他使用国内 DNS

下载(releases): [https://github.com/xream/AdGuardHomeUpstreamDnsFile/releases/latest/download/global.txt](https://github.com/xream/AdGuardHomeUpstreamDnsFile/releases/latest/download/global.txt)

下载(branch): [https://raw.githubusercontent.com/xream/AdGuardHomeUpstreamDnsFile/release/global.txt](https://raw.githubusercontent.com/xream/AdGuardHomeUpstreamDnsFile/release/global.txt)

数据来源: [blackmatrix7/ios_rule_script Global](https://github.com/blackmatrix7/ios_rule_script/tree/master/rule/Surge/Global)

格式参考 `china.txt`

一个例子, 仅供参考, 可自行修改, 设为定时任务:

```bash
#!/usr/bin/env bash
# > 下载文件到 /opt/adguardhome/conf/global.txt
curl -L https://github.com/xream/AdGuardHomeUpstreamDnsFile/releases/latest/download/global.txt -o /opt/adguardhome/conf/global.txt
# > 修改文件内容
# > 分流到 国外 DNS
sed -i "s|__CHINA_DNS_PLACEHOLDER__|tls://9.9.9.11 tls://208.67.220.220 tls://8.8.4.4|g" /opt/adguardhome/conf/global.txt
sed -i "s|__GLOBAL_DNS_PLACEHOLDER__|tls://9.9.9.11 tls://208.67.220.220 tls://8.8.4.4|g" /opt/adguardhome/conf/global.txt
# > 其他分流到 国内 DNS
sed -i "1s|^|https://doh-pure.onedns.net/dns-query\nhttps://120.53.53.53/dns-query\nhttps://223.6.6.6/dns-query\n|" /opt/adguardhome/conf/global.txt
```

修改 `/opt/adguardhome/conf/AdGuardHome.yaml` 中的 `upstream_dns_file` 为 `/opt/adguardhome/conf/global.txt`

重启 AdGuard Home
