
#### 操作步骤

```
yum install wget
wget https://pkg.cfssl.org/R1.2/cfssl_linux-amd64
wget https://pkg.cfssl.org/R1.2/cfssljson_linux-amd64
wget https://pkg.cfssl.org/R1.2/cfssl-certinfo_linux-amd64
ls
mv cfssl* /data/workspace/src/bk-bcs/credial/
chmod -x cfssl*
cd  /data/workspace/src/bk-bcs/credial/
chmod -x cfssl*
for x in cfssl*; do mv $x ${x%*_linux-amd64};  done
mv cfssl* /usr/bin
cfssl gencert --initca ca-csr.json | cfssljson -bare bcs-ca
chmod -x /usr/bin/cfssl
chmod -x /usr/bin/cfssljson
cfssl gencert --initca ca-csr.json | cfssljson -bare bcs-ca
chmod -777 /usr/bin/cfssljson
chmod -777 /usr/bin/cfssl
cfssl gencert --initca ca-csr.json | cfssljson -bare bcs-ca
chmod a+x /usr/bin/cfssljson
chmod a+x /usr/bin/cfssl
cfssl gencert --initca ca-csr.json | cfssljson -bare bcs-ca
cfssl gencert -ca=bcs-ca.pem   -ca-key=bcs-ca-key.pem   -config=ca-config.json   -profile=client   ca-csr.json | cfssljson -bare bcs-client
cfssl gencert -ca=bcs-ca.pem   -ca-key=bcs-ca-key.pem   -config=ca-config.json   -profile=server   ca-csr.json | cfssljson -bare bcs-server
cfssl gencert --initca ca-csr.json | cfssljson -bare etcd-ca
cfssl gencert -ca=etcd-ca.pem   -ca-key=etcd-ca-key.pem   -config=ca-config.json   -profile=client   ca-csr.json | cfssljson -bare etcd-client
cfssl gencert -ca=etcd-ca.pem   -ca-key=etcd-ca-key.pem   -config=ca-config.json   -profile=server   ca-csr.json | cfssljson -bare etcd-server
history


```