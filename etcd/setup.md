

### 1、下载好安装包，解压当前文件夹

```
tar -zxvf etcd-v3.5.5-linux-amd64.tar.gz
```


### 2、复制etcd命令行工具到/usr/bin目录下

```
cp etcd-v3.5.5-linux-amd64/etcd* /usr/bin
```



### 3、刷新配置文件

```
mkdir /data/etcd
cd /data/etcd
vi etcd.conf.yaml

name: "etcd-node"
data-dir: "/data/etcd"
listen-peer-urls: "http://0.0.0.0:2380"
listen-client-urls: "http://0.0.0.0:2379"
advertise-client-urls: "http://127.0.0.1:2379"



```






### 4、启动etcd

```
nohup etcd --config-file=/data/etcd/etcd.conf.yaml 
```

#### nohup解析

```
nohup 英文全称 no hang up（不挂起），用于在系统后台不挂断地运行命令，退出终端不会影响程序的运行。

nohup 命令，在默认情况下（非重定向时），会输出一个名叫 nohup.out 的文件到当前目录下，如果当前目录的 nohup.out 文件不可写，输出重定向到 $HOME/nohup.out 文件中。
```

