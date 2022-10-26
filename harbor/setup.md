# Harbor离线安装

## harbor离线安装

### 安装环境及版本如下：

- 系统centos7.9
- doker版本docker-ce-20.10.21
- docker-compose版本v2.12.2
- harbor版本v2.6.1
- harbor数据路径/data/harbor
- 访问域名harbor.domain.com


### 预安装docker和docker-compose

### 安装harbor

#### 下载harbor安装包

在页面https://github.com/goharbor/harbor/releases下载对于版本包，这里以v2.6.1为例,

harbor-offline-installer-v2.6.1.tgz

#### 修改hosts

```
vim /etc/hosts

127.0.0.1 harbor.domain.com
127.0.0.1 harbor.domain
127.0.0.1 harbor
```

#### 配置https证书

##### 生成证书颁发机构证书

```
openssl genrsa -out ca.key 4096
```

```
openssl req -x509 -new -nodes -sha512 -days 3650 \
-subj "/C=CN/ST=Guangdong/L=Shenzhen/O=greene/OU=Personal/CN=harbor.domain.com" \
-key ca.key \
-out ca.crt
```

```
openssl genrsa -out harbor.domain.com.key 4096
```




##### 生成服务器证书

```
openssl req -sha512 -new \
-subj "/C=CN/ST=Guangdong/L=Shenzhen/O=greene/OU=Personal/CN=harbor.domain.com" \
-key harbor.domain.com.key \
-out harbor.domain.com.csr
```

```
cat > v3.ext <<-EOF
authorityKeyIdentifier=keyid,issuer
basicConstraints=CA:FALSE
keyUsage = digitalSignature, nonRepudiation, keyEncipherment, dataEncipherment
extendedKeyUsage = serverAuth
subjectAltName = @alt_names

[alt_names]
DNS.1=harbor.domain.com
DNS.2=harbor.domain
DNS.3=harbor
EOF

```

```
openssl x509 -req -sha512 -days 3650 \
-extfile v3.ext \
-CA ca.crt -CAkey ca.key -CAcreateserial \
-in harbor.domain.com.csr \
-out harbor.domain.com.crt
```

##### 向Harbor和Docker提供证书

```
cp harbor.domain.com.crt /etc/ssl/certs/
cp harbor.domain.com.key /etc/ssl/certs/
```

```
openssl x509 -inform PEM -in harbor.domain.com.crt -out harbor.domain.com.cert
```

```
mkdir -p /etc/docker/certs.d/harbor.domain.com
cp harbor.domain.com.cert /etc/docker/certs.d/harbor.domain.com/
cp harbor.domain.com.key /etc/docker/certs.d/harbor.domain.com/
cp ca.crt /etc/docker/certs.d/harbor.domain.com/
```

##### 重启docker
```
systemctl restart docker
```

#### 配置harbor启动

```
tar -xvf harbor-offline-installer-v2.6.0.tgz
cd harbor
cp harbor.yml.tmpl harbor.yml
```

```
vim harbor.yml
```

##### 修改如下配置即可

```
hostname: harbor.domain.com
https:
  certificate: /etc/ssl/certs/harbor.domain.com.crt
  private_key: /etc/ssl/certs/harbor.domain.com.key
data_volume: /data/harbor

```

##### 开始安装并启动harbor

```
./install.sh
```


登录验证，Harbor的用户和默认密码是admin/Harbor12345

#### docker客户端配置

创建证书目录，将上述harbor服务端的ca.crt证书拷贝到目录下

```
mkdir -p /etc/docker/certs.d/harbor.domain.com
cp /opt/harbor/harbor.domain.com.crt   /etc/docker/certs.d/harbor.domain.com
如果不对就多复制几个 吧这些密钥文件都拷贝过去
```


重启客户端docker即可

```
systemctl restart docker
```


