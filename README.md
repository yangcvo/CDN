## CDN的初级知识考核

![](http://7xrthw.com1.z0.glb.clouddn.com/cdn.mmtrix.png)


个人博客： blog.yancy.cc

个人从业系统运维，自动化运维，应用运维，虚拟化运维，大数据运维，行业。

我15年在高升控股实习，主要做云加速，云评测`mmtrix`公司待过，自己整理下所学的CDN初级需要会的。
主要负责面试考核这块新招员工。


下面是我当初出的题目，笑纳了。



#### 1.CDN 请解释一下什么事CDN以及作用。
```
答:CDN 是内容分发网络。 Content delivery network  作用：1 提高企业站点访问速度，大大提升性能站点的稳定性.
降低各种  D.D.o.S攻击对网站的影响 
减轻主站点服务器负载，实现异地网络加速.
```

#### 2.请阐述下一位用户在浏览器访问一个域名到呈现出网页的内容过程。

```
答：通过输入访问域名。要找出其IP地址，先在本地hosts下面查找有没有这个解析记录。如果没有路由器接着在向
DNS服务器发起解析请求。DNS服务器递归查询，找到该域名对应A记录解析。然后将该解析记录返回浏览器和源站建立连接，源站内容就返回给用户浏览器呈现出来。
```

#### 3.请阐述下一个新用户接入我们CDN平台所需要做的操作哪些？

```
 
 ( 1） 写bind 和squid 的缓存配置文件
（ 2） 找对应的域名URL
 ( 3）上测试机测试是否同步成功
（ 4）配置cname-DNSPOD
 ( 5) 测试脚步测试URL是否200ok
 ( 6) 解析下cname
 ( 7)提交cname
（ 8） 整理出来表格 上传SVN上面

```

####  4.列出CDN节点中重要的应用名称和作用。

```
（1） 安装 log_agent   日志监控。
                monitor_api_agent 安装秒级监控
                LotServer  对tcp网卡的优化加速。

                 squid.py 配置文件  内容URL缓存

              bind.py 配置文件   域名解析
               salt下发  查看内容是否一致。
```

#### 5. 默认最小化centos 7.1系统，我在rc.local 文件中插入了如下一条命令setenfroce 请问重启后系统的selinux的状态是什么？为什么？

```
答：状态关闭selinux防火墙。 因为setenfroce 0 是关闭防火墙意思。set （设置） enfroce (执行 )放在rc.local 系统重启会执行这个最后一个程序，就会生效，当开机的时候就已经执行了这条命令。
```



### 6.一台centos 7系统的服务器，我想要开放默认的80端口。该如何操作？我们现在设备使用的是那个防护策略。
```
答：写条防火墙规则： 关闭防火墙一样可以生效 写在配置文件里：
-A -INPUT -m state --state NEW -m -p tcp  --dport 80 -j ACCEPT
```

#### 7.有一台centos 7的系统服务器 在/etc/resolv.conf 文件设置了一个114 的DNS地址，重启设备以后发现DNS不见了。这是为什么？该如何解决。

```
出现重启不见，解决方案：Network Manager 关闭掉。

* chkconfig Network Manager off      
* chkconfig Network no
* service Network Manager stop
* service Network start

```
#### 8.如果开启linux 系统中ip forward 请写出详细的操作步骤。

```
 IP forward: echo "1" > /proc/sys/net/ipv4/ip_forward
```
#### 9. 写下面计划任务
```
 （1）在下午5：50 删除/123 目录下全部子目录和全部文件
   50 5 * * * rm /123 -rf
 ( 2 ) 从早9：00 下午5：00每小时读取/456目录下x1文件每行第一个域的全部数据加入到/bak目录下的bak01.txt文件内；
   9-17/1 * * *　cat/456/x1/ | awk -F " '{print $1}' >/bak/bak01.txt
  (3)每逢星期一下午4：50将/data/目录下的所有目录和文件归档并压缩为文件：backup.tar.gz
   50 16 * * 1  tar zcvf backup.tar.gz /data
  (4) 在下午4：55将IDE 接口的CD-ROM卸载 （假设设备名为HDC）
   55 16 * * * umount /dev/hdc
```

#### 10.简要说明http 403.404.502 错误的原因和解决方法。
```
 403 是没有访问权限。是服务器权限问题，需要查看是不是有读的权限。或者证书过期。
 404 网页不存在。    是服务器的网页页面路径不正确。   
 502   网关或代理服务器时收到无效的响应 。
一般出现了这个问题是由于不良的IP之间的沟通后端计算机，包括您可能尝试访问的在Web服务器上的网站。在分析这个问题，您应该先完全清除浏览器缓存
```

#### 11.saltstack 中几个重要模块，如配置文件比对，下发等，请对以下操作写出具体命令。

```
vimdiff bind.py bind.py.bak150701 对比

 对比  salt -v -N "test" saltutil.sync_grains

 下发 salt -v -N "test" state.sls bind >new_group.bind.1
```

#### 12. 使用salt master 如配置文件比对，下发等，请对以下操作写出具体命令。

```
（1）什么情况才下发。
( 2 ) 如何获取线上全部节点的IP地址。
新用户上来，配置文件，这个时候才同步下发。
```
