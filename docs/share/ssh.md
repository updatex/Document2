Xin, Gen 2018-10-09
### ssh-keygen
* type : rsa
* 可以指定产生的目录，默认~/.ssh/
* passprase 这个用来加密private-key


### ubuntu 如何支持root 远程登陆
* 默认不支持
> https://jingyan.baidu.com/article/e6c8503c724ebae54f1a18cd.html

### .ssh/　下目录结构
* authorized_keys
> 存储public-key。主机使用private-key连接时候，免密钥登陆使用。
* known_hosts
> ssh会把你每个你访问过计算机的公钥(public key)都记录在~/.ssh/known_hosts。而这个是主动了，我登陆你，你的pub留在我这里。

### cat pub file.

ssh -p 11004 gxin@127.0.0.1 'mkdir -p .ssh & cat >> .ssh/authorized_keys' < ~/.ssh/id_rsa.pub

### 端口
* default 22
* -p <port>


### 绑定本地端口
* ssh -D <port> user@host
> ssh 建立一个socket ，监听<port> 一旦有数据传向，自动转移到ssh上。
### 端口转发（ssh tunnel,ssh 通道)
> 个人理解，借助第三方server，连接ssh client and ssh server，可能是单向，可能是反向

* local forwarding 本地端口转发
```
ssh -L <local_port>:target_server:<target_port1> proxy_server

这个命令运行在local上，创建了一个local to target的通道。这个时候proxy_server就像消失了，只需要关注local和target
example: ssh -L 2121:host2:21 host3
ftp localhost:2121
! -L 可以多个，可以借此使用类似putty
```

* remote forwarding

```
ssh -R <local_listing_port>:target_server:<target_port> source_server
这个命令运行在proxy_server上，创建source to target 的通道。
proxy_server需要一直监视listing_port来自local的数据，并且转发到target_server
example:
ssh -R 2121:host2:21 host1 (create host1 to host2 tunnel on host3)
ftp localhost:2121
```

> remote forwarding 一般是 host1 host2 host3, host2 可以连接host1, host3
但是host1不能连接host2，同时host1又希望连接host3。这个时候可以考虑运行在host2上创建通道。　所以无论local or remote forwarding，都是需要连接本地端口


### ssh options
* keep-avlie 
```
 -o ConnectTimeout=<secs>
 -o ServerAliveInterval=<secs>
 -o ServerAliveCountMax=<num>
 
```

### ssh configure
* ~/.ssh/config (single user)
* /etc/ssh/sshd_config (all user)
