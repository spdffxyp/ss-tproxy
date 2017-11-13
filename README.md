## ss-redir + iptables 透明代理/一键脚本
> 
使用之前，请先安装 shadowsocks-libev、chinadns、dnsforwarder、ipset！！！
如果你不知道如何安装，请前往 [ss-redir透明代理 - 安装相关组件](https://www.zfl9.com/ss-redir.html#安装相关组件)。

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
4. `ss-tproxy.conf`配置文件其实就是一个 shell 脚本，具体怎么玩，就随你们自己咯。

**获取 ipset-chinaip 大陆地址段**
1. `ss-tproxy update_ipset`
2. 默认保存在`/etc/ipset.chinaip`文件
3. `ipset -L chinaip`，查看大陆地址段

**开启 ip_forward 内核网卡转发功能**
1. 查看启用状态：`ss-tproxy show_ipfwd`，如果为 enabled，说明已开启；
2. 如果为 disabled，请自行添加`net.ipv4.ip_forward = 1`至`/etc/sysctl.conf`文件，并执行`sysctl -p`生效。

## ss-tproxy - 用法详解
**start|stop|status|restart**
1. `ss-tproxy start`，运行 ss-tproxy；
2. `ss-tproxy status`，打印 ss-tproxy 运行状态；
3. `ss-tproxy stop`，停止 ss-tproxy；
4. `ss-tproxy restart`，重启 ss-tproxy；

**其他参数，请使用`ss-tproxy help`查看**
