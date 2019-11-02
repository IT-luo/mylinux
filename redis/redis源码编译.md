# Redis源码编译安装手册

### 下载源码包

1、下载Redis源码包，这里使用的是redis-5.0.3、官网下载地址：<http://download.redis.io/releases/>

``` markdown
[root@node1 /usr/local/src]#wget http://172.20.7.53/yum/redis/redis-5.0.3.tar.gz
```

### 解压软件包

1、把下载好的redis源码包解压到当前目录中

```markdown
[root@node1 /usr/local/src]#tar xf redis-5.0.3.tar.gz
```

2、编译Redis,需要加上PREFIX=/path/ 指定安装路径，不值得的话默认是在当前目录下中的src里

```mariadb
[root@node1 /usr/local/src/redis-5.0.3]#make PREFIX=/usr/local/redis install 
```

### 修改配置文件

1、先从解压路径下把redis.conf复制到redis安装目录下

```markdown
[root@node1 ~]#cp /usr/local/src/redis-5.0.3/redis.conf  /usr/local/redis/
```

2、修改redis.conf配置文件

```markdown
[root@node1 /usr/local/redis]#vim redis.conf 
bind 172.20.7.50
daemonize yes
pidfile /usr/local/redis/redis_6379.pid
logfile "/usr/local/redis/redis_6379.log"
databases 16
stop-writes-on-bgsave-error no  # 当Redis快照时错误是否停止往里写数据 默认yes，改成no
dbfilename redis_6379.rdb		# 快照名称
dir /usr/local/redis            # 快照保存路径
requirepass 123456              # 设置Redis密码
maxclients 10000				# 客户端最大连接数
maxmemory 2147483648			# 最大允许redis使用多大内存这里使用字节为单位
appendonly yes					# Redis aof文件相当于mysql的binlog日志文件
appendfilename "appendonly.aof" 
auto-aof-rewrite-percentage 100
auto-aof-rewrite-min-size 64mb
requirepass 123456
# 手动启动时有三个警告
[root@node1 /usr/local/redis]#vim /etc/sysctl.conf
net.core.somaxconn = 512
vm.overcommit_memory = 1
# 开启大页内存动态分配，学要关闭iptables，由于重启之后失效，加到/etc/rc.local中去实现自动执行
[root@node1 /usr/local/redis]#echo never > /sys/kernel/mm/transparent_hugepage/enabled

# scp拷贝内核优化文件到其他两台服务器
[root@node1 /usr/local/redis]#scp /etc/sysctl.conf 172.20.7.52:/etc/sysctl.conf
[root@node1 /usr/local/redis]#scp /etc/sysctl.conf 172.20.7.56:/etc/sysctl.conf
```

### 编辑service启动文件

1、配置Redis的service启动文件

```markdown
[root@node1 /usr/local/redis]#vim /usr/lib/systemd/system/redis.service
[Unit]
Description=Redis persistent key-value database
After=network.target
After=network-online.target
Wants=network-online.target

[Service]
ExecStart=/usr/local/redis/bin/redis-server /usr/local/redis/etc/redis.conf  --supervised systemd
ExecReload=/bin/kill -s HUP $MAINPID 
ExecStop=/bin/kill -s QUIT $MAINPID
Type=notify
User=redis
Group=redis
RuntimeDirectory=redis
RuntimeDirectoryMode=0755

[Install]
WantedBy=multi-user.target
```

### 创建所需要的目录和用户

1、创建配置文件的存放目录

```markdown
[root@node1 /usr/local/redis]#mkdir etc
[root@node1 /usr/local/redis]#mv /usr/local/src/redis.conf /usr/local/redis/etc/
```

2、创建redis 用户用来启动redis

```markdown
[root@node1 /usr/local/redis]#useradd redis -s /sbin/nologin
```

3、把/usr/local/redis/bin/* 创建软连接到/usr/sbin下

```markdown
[root@node1 /usr/local/redis]#ln -sv /usr/local/redis/bin/* /usr/sbin/
‘/usr/sbin/redis-benchmark’ -> ‘/usr/local/redis/bin/redis-benchmark’
‘/usr/sbin/redis-check-aof’ -> ‘/usr/local/redis/bin/redis-check-aof’
‘/usr/sbin/redis-check-rdb’ -> ‘/usr/local/redis/bin/redis-check-rdb’
‘/usr/sbin/redis-cli’ -> ‘/usr/local/redis/bin/redis-cli’
‘/usr/sbin/redis-sentinel’ -> ‘/usr/local/redis/bin/redis-sentinel’
‘/usr/sbin/redis-server’ -> ‘/usr/local/redis/bin/redis-server’
```

4、把/usr/local/redis目录以及子目录权限全修改为redis所有者

```markdown
[root@node1 /usr/local]#chown -R redis.redis redis/
```

### 启动Redis服务

1、上述操作配置完成之后就可以启动Redis

```markdown
[root@node1 /usr/local/redis]#systemctl start redis
[root@node1 /usr/local/redis]#ss -tnl 
State      Recv-Q Send-Q               Local Address:Port              Peer Address:Port              
LISTEN     0      511                          *:6379                         *:*                  
LISTEN     0      128                          *:111                          *:*                  
LISTEN     0      128                          *:22                           *:*            
```

### 使用客户端连接命令进入到redis服务端

1、使用redis-cli命令连接

```markdown
[root@node1 /usr/local]#redis-cli 
127.0.0.1:6379> KEYS *
(empty list or set)
127.0.0.1:6379> info
# Server
redis_version:5.0.3
redis_git_sha1:00000000
redis_git_dirty:0
redis_build_id:9b187def1f76bf90
redis_mode:standalone
os:Linux 3.10.0-862.el7.x86_64 x86_64
arch_bits:64
multiplexing_api:epoll
atomicvar_api:atomic-builtin
gcc_version:4.8.5
process_id:45127
run_id:0537fe7538b5468509159bb179e4fdc7eab2d15e
tcp_port:6379

```

