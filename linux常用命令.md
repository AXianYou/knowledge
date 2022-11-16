## Centos7：

- 离线进行安装
  -  rpm -ivh 包名
- 查看文件进程状态
  - systemctl status zabbix-agent.service
- 启动文件服务
  - systemctl start zabbix-agent.service
- 设置文件自启动
  - systemctl enable zabbix-agent.service
- 查看进程状态
  - ps aux | grep zabbix
- 查看端口状态
  - netstat -alnptu | grep 10050    （可以使用端口或者进程名）
- 防火墙相关
  - 开放的端口：firewall-cmd --zone=public --add-port=10051/tcp --permanent
  - systemctl restart firewalld            systemctl status firewalld    
  - 重新加载防火墙配置（非重启） ： firewall-cmd --reload

## Centos6.5：

- 查看防火墙状态
  - service iptables status

- 停止防火墙
  - service iptables stop

- 启动防火墙
  - service iptables start

- 重启防火墙
  - service iptables restart

- 永久关闭防火墙
  - chkconfig iptables off
- 永久关闭后重启
  - chkconfig iptables on
- 开放端口：
  - vi /etc/sysconfig/iptables
  - -I INPUT -m state --state NEW -m tcp -p tcp --dport 80 -j ACCEPT
    COMMIT

//保存退出后先重启防火墙
service iptables restart
//后保存防火墙
service iptables save

执行顺序:从上往下执行，遇到匹配的就不执行下面的规则
插入顺序：-A append的意思，是在规则的末尾加一条规则，-I insert的意思，是在规则的开头加一条数据
INPUT：进站请求【出站OUTPUT】
-p：protocol，指定协议（icmp/tcp/udp）
--dport：指定端口号
-j：指定行为结

1. 只允许x.x.x.x访问本机
iptables -I INPUT -p tcp -j DROP #若要添加多了ip，该条也是只执行一次
iptables -I INPUT -s x.x.x.x -p tcp -j ACCEPT
2. 只允许x.x.x.x访问本机的xx端口
iptables -I INPUT -p tcp --dport xx -j DROP #若要添加多了端口，该条也是只执行一次
iptables -I INPUT -s x.x.x.x -p tcp --dport xx -j ACCEPT
3. 只允许x.x.x.x网段访问本机
iptables -I INPUT -p tcp -j DROP #若要添加多了网段，该条也是只执行一次
iptables -I INPUT -s x.x.x.0/24 -p tcp -j ACCEPT
4. 只禁止x.x.x.x访问本机
iptables -I INPUT -p tcp -j ACCEPT #若要添加多了ip，该条也是只执行一次
iptables -I INPUT -s x.x.x.x -p tcp -j DROP