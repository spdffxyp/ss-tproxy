## ss-redir 透明代理/脚本
- 请先安装 [shadowsocks-libev](https://github.com/shadowsocks/shadowsocks-libev)、[chinadns](https://github.com/shadowsocks/ChinaDNS)、[dnsforwarder](https://github.com/holmium/dnsforwarder)、ipset！
- 如果你不知道如何安装，请前往 [ss-redir透明代理 - 安装相关组件](https://www.zfl9.com/ss-redir.html#安装相关组件)。
- 并且，强烈建议先阅读 [ss-redir透明代理](https://www.zfl9.com/ss-redir.html)，不然，你会觉得一头雾水。

## ss-tproxy - 初始化
**获取一键脚本**
1. `git clone https://github.com/zfl9/ss-tproxy.git`
2. `cd ss-tproxy/ && chmod +x ss-tproxy`
3. `cp -af ss-tproxy /usr/local/bin/`
4. `cp -af ss-tproxy.conf /etc/ss-tproxy.conf`

**修改配置文件**
1. 只需修改`服务器信息`，其他的保持默认即可；如果需要修改其他参数，文件中有详细注释可参考。
2. `vim /etc/ss-tproxy.conf`，修改开头的"服务器信息"：服务器地址、服务器端口、加密方式、账户密码。
3. **服务器地址**可以为 IP，也可以为域名；如果是域名，务必将域名和 IP 的解析关系添加至`/etc/hosts`文件！
4. `ss-tproxy.conf`配置文件其实就是一个 shell 脚本，具体怎么玩，可以自由发挥。

**配置开机自启**
- RHEL/CentOS 6.x 及其它使用 sysvinit 的发行版
 1. 使用`/etc/rc.d/rc.local`文件
 2. `echo "/usr/local/bin/ss-tproxy start" >> /etc/rc.d/rc.local`
- RHEL/CentOS 7.x 及其它使用 systemd 的发行版
 1. 安装 ss-tproxy.service 服务
 2. `cp -af ss-tproxy.service /etc/systemd/system/ && systemctl daemon-reload && systemctl enable ss-tproxy`

## ss-tproxy - 参数
1. `ss-tproxy start`，运行 ss-tproxy；
2. `ss-tproxy status`，ss-tproxy 运行状态；
3. `ss-tproxy stop`，停止 ss-tproxy；
4. `ss-tproxy restart`，重启 ss-tproxy；
5. `ss-tproxy flush_dnsfwd`，清空 dnsforwarder dns缓存
6. `ss-tproxy update_chnip`，更新 ipset-chnip 大陆地址段

## ss-tproxy - 关于
- author：Otokaze
- url：https://www.zfl9.com
- ref: https://www.zfl9.com/ss-redir.html
- date: 2017-11-13 13:23:35 CST
