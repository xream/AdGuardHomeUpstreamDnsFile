# AdGuard Home upstream_dns_file

基于域名的简单分流 不完美 按需使用

## 使用方法

### `china.txt`

下载: [https://github.com/xream/AdGuardHomeUpstreamDnsFile/releases/latest/download/china.txt](https://github.com/xream/AdGuardHomeUpstreamDnsFile/releases/latest/download/china.txt)

数据来源: [blackmatrix7/ios_rule_script ChinaMaxNoIP](https://github.com/blackmatrix7/ios_rule_script/tree/master/rule/Surge/ChinaMaxNoIP)

格式:

```
# > 来自 DOMAIN-SUFFIX,xiaoma.io
# > 表示 xiaoma.io, *.xiaoma.io 全部分流到 __CHINA_DNS_PLACEHOLDER__
[/xiaoma.io/zzq12345.gearhostpreview.com/]__CHINA_DNS_PLACEHOLDER__

# > 来自 DOMAIN,zzq12345.gearhostpreview.com
# > 表示 仅 zzq12345.gearhostpreview.com 分流到 __CHINA_DNS_PLACEHOLDER__
# > 在上面生成一条 zzq12345.gearhostpreview.com 分流到 __CHINA_DNS_PLACEHOLDER__
# > 在这里生成一条 *.zzq12345.gearhostpreview.com 分流到 __GLOBAL_DNS_PLACEHOLDER__
[/*.zzq12345.gearhostpreview.com/]__GLOBAL_DNS_PLACEHOLDER__
```

`__CHINA_DNS_PLACEHOLDER__` 和 `__GLOBAL_DNS_PLACEHOLDER__` 自行替换即可

在实际使用中, 可能应当将本文件的两者都替换成 `国内 DNS`

一个例子:

```bash
#!/usr/bin/env bash
# > 下载文件到 /opt/adguardhome/conf/china.txt
curl -L https://github.com/xream/AdGuardHomeUpstreamDnsFile/releases/latest/download/china.txt -o /opt/adguardhome/conf/china.txt
# > 修改文件内容
# > 分流到 国内 DNS
sed -i '' "s|__CHINA_DNS_PLACEHOLDER__|https://doh-pure.onedns.net/dns-query https://120.53.53.53/dns-query https://223.6.6.6/dns-query|g" /opt/adguardhome/conf/china.txt
sed -i '' "s|__GLOBAL_DNS_PLACEHOLDER__|https://doh-pure.onedns.net/dns-query https://120.53.53.53/dns-query https://223.6.6.6/dns-query|g" /opt/adguardhome/conf/china.txt
# > 其他分流到 国外 DNS
sed -i '' "1s|^|tls://9.9.9.11\ntls://208.67.220.220\ntls://8.8.4.4\n|" /opt/adguardhome/conf/china.txt
```

修改 `/opt/adguardhome/conf/AdGuardHome.yaml` 中的 `upstream_dns_file` 为 `/opt/adguardhome/conf/china.txt`

重启 AdGuard Home

### `global.txt`

下载: [https://github.com/xream/AdGuardHomeUpstreamDnsFile/releases/latest/download/global.txt](https://github.com/xream/AdGuardHomeUpstreamDnsFile/releases/latest/download/global.txt)

数据来源: [blackmatrix7/ios_rule_script Global](https://github.com/blackmatrix7/ios_rule_script/tree/master/rule/Surge/Global)

格式参考 `china.txt`

`__CHINA_DNS_PLACEHOLDER__` 和 `__GLOBAL_DNS_PLACEHOLDER__` 自行替换即可

一个例子:

```bash
#!/usr/bin/env bash
# > 下载文件到 /opt/adguardhome/conf/global.txt
curl -L https://github.com/xream/AdGuardHomeUpstreamDnsFile/releases/latest/download/global.txt -o /opt/adguardhome/conf/global.txt
# > 修改文件内容
# > 分流到 国内 DNS
sed -i '' "s|__CHINA_DNS_PLACEHOLDER__|tls://9.9.9.11 tls://208.67.220.220 tls://8.8.4.4|g" /opt/adguardhome/conf/global.txt
sed -i '' "s|__GLOBAL_DNS_PLACEHOLDER__|tls://9.9.9.11 tls://208.67.220.220 tls://8.8.4.4|g" /opt/adguardhome/conf/global.txt
# > 其他分流到 国外 DNS
sed -i '' "1s|^|https://doh-pure.onedns.net/dns-query\nhttps://120.53.53.53/dns-query\nhttps://223.6.6.6/dns-query\n|" /opt/adguardhome/conf/global.txt
```

修改 `/opt/adguardhome/conf/AdGuardHome.yaml` 中的 `upstream_dns_file` 为 `/opt/adguardhome/conf/global.txt`

重启 AdGuard Home
