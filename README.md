## ss-redir + iptables 透明代理/一键脚本
使用之前，请先安装 [shadowsocks-libev](https://github.com/shadowsocks/shadowsocks-libev)、[chinadns](https://github.com/shadowsocks/ChinaDNS)、[dnsforwarder](https://github.com/holmium/dnsforwarder)、ipset！！！
如果你不知道如何安装，请前往 [ss-redir透明代理 - 安装相关组件](https://www.zfl9.com/ss-redir.html#安装相关组件)。
并且，强烈建议先阅读 [ss-redir透明代理](https://www.zfl9.com/ss-redir.html)，不然，你会觉得一头雾水。

## ss-tproxy - 初始化步骤
**获取 ss-tproxy 脚本及其配置文件**
1. `git clone https://github.com/zfl9/ss-tproxy.git`
2. `cd ss-tproxy/ && chmod +x ss-tproxy`
3. `cp -af ss-tproxy /usr/local/bin/`
4. `cp -af ss-tproxy.conf /etc/ss-tproxy.conf`

**修改 ss-tproxy.conf 配置文件**
1. 只需修改`服务器信息`，其他的保持默认即可；如果需要修改其他参数，文件中有详细注释可参考。
2. `vim /etc/ss-tproxy.conf`，修改开头的"服务器信息"：服务器地址、服务器端口、加密方式、账户密码。
3. **服务器地址**可以为 IP，也可以为 域名；如果是域名，务必将域名和 IP 的解析关系添加至`/etc/hosts`文件！
4. `ss-tproxy.conf`配置文件其实就是一个 shell 脚本，具体怎么玩，可以自由发挥。

**配置 ipset-chinaip 大陆地址段**
1. `ss-tproxy update_ipset`
2. 默认保存在`/etc/ipset.chinaip`文件
3. `ss-tproxy show_ipset`，查看大陆地址段

**配置 iptables-shadowsocks 规则**
1. `ss-tproxy update_ipts`
2. 默认保存在`/etc/iptables.shadowsocks`文件
3. `ss-tproxy show_ipts`，查看 iptables 规则

**开启 ip_forward 内核网卡转发功能**
1. 查看启用状态：`ss-tproxy show_ipfwd`，如果为 enabled，说明已开启；
2. 如果为 disabled，请自行添加`net.ipv4.ip_forward = 1`至`/etc/sysctl.conf`文件，并执行`sysctl -p`生效。

## ss-tproxy - 参数详解
**start|stop|status|restart**
1. `ss-tproxy start`，运行 ss-tproxy；
2. `ss-tproxy status`，ss-tproxy 运行状态；
3. `ss-tproxy stop`，停止 ss-tproxy；
4. `ss-tproxy restart`，重启 ss-tproxy；

**其他参数**
`show_ipset`：查看 ipset-chinaip 大陆地址段
`clear_ipset`：清空 ipset-chinaip 大陆地址段
`restore_ipset`：恢复 ipset-chinaip 大陆地址段 from 'ipset.chinaip'
`update_ipset`：更新 ipset-chinaip 大陆地址段

`show_ipts`：查看 iptables-shadowsocks 规则
`clear_ipts`：清空 iptables-shadowsocks 规则
`restore_ipts`：恢复 iptables-shadowsocks 规则 from 'iptables.shadowsocks'
`update_ipts`：更新 iptables-shadowsocks 规则

`show_rule`：查看 PRB 策略路由规则
`clear_rule`：清空 PBR 策略路由规则
`restore_rule`：设置 PBR 策略路由规则

`chk_file`：检查'chinadns_chnroute.txt'、'chinadns_iplist.txt'、'ipset.chinaip'、'iptables.shadowsocks'文件是否存在

`show_dnsfwd`：查看 dnsforwarder/config 配置文件
`flush_dnsfwd`：清空 dnsforwarder/cache dns缓存
`update_dnsfwd`：更新 dnsforwarder/config 配置文件，并重启 dnsforwarder 使其生效

`show_dns`：查看当前系统 dns 服务器设置
`edit_dns`：编辑当前系统 dns 服务器设置 '/etc/resolv.conf'

`show_ipfwd`：查看内核网卡转发功能是否已开启
`edit_ipfwd`：编辑 '/etc/sysctl.conf' 配置文件，并执行 'sysctl -p' 使其生效

## ss-tproxy - 开机自启
- CentOS 6.x
 1. `vim /etc/rc.d/rc.local`，修改 rc.local 文件
 2. 添加 `ss-tproxy start` 行，保存退出即可
- CentOS 7.x 及 ArchLinux
 1. 因为使用 systemd 代替了 sysvinit，因此稍微复杂一些
 2. 配置自定义 rc-local.service 系统服务，`vim /etc/systemd/system/rc-local.service`，内容如下：
``` bash
[Unit]
Description=/etc/rc.local
ConditionPathExists=/etc/rc.local
After=systemd-networkd.service sshd.service

[Service]
Type=forking
ExecStart=/etc/rc.local
RemainAfterExit=yes

[Install]
WantedBy=multi-user.target
```
 3. 保存退出，将 rc-local.service 加入开机自启：`systemctl daemon-reload && systemctl enable rc-local`
 4. 新建 /etc/rc.local 文件，并设置可执行权限：`touch /etc/rc.local && chmod +x /etc/rc.local`
 5. 添加 `ss-tproxy start`行，保存退出即可
