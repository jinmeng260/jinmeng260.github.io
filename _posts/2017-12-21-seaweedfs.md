---
layout: post
title:  seaweedfs
tags:   server filesystem
category:   server filesystem
---

# seaweedfs

seaweedfs��һ���� golang �����ķֲ�ʽ�洢�ļ���ϵͳ, �����ڴ洢����С�ļ���

## ��װ
```shell

# install golang

wget https://dl.gocn.io/golang/1.9.2/go1.9.2.linux-amd64.tar.gz
sudo tar -C /usr/local -zxvf go1.9.2.linux-amd64.tar.gz

mkdir -p ~/go/src
echo "export GOPATH=$HOME/go" >> ~/.bashrc
echo "export PATH=$PATH:$GOPATH/bin:/usr/local/go/bin" >> ~/.bashrc
source ~/.bashrc
go version

# install mercurial

sudo apt install git mercurial

# install seaweedfs
#go get github.com/chrislusf/seaweedfs/go/weed

wget  https://github.com/chrislusf/seaweedfs/releases/download/0.76/freebsd_amd64.tar.gz
sudo tar -C /usr/local/bin/ freebsd_amd64.tar.gz

```

## ʹ��

��������

```shell
# ����������: localhost:9333
./weed master
# ���������:
./weed volume -max=100 -mserver="localhost:9333"

# ����

./weed server -master.port=9333 -volume.port=8080 -dir="/tmp/data"

```

�ϴ��ļ�

```
curl -F file=@/tmp/1.jpg http://localhost:9333/submit [15:19:42]
>{"fid":"6,61db2c0e36","fileName":"1.jpg","fileUrl":"192.168.100.2:8080/6,61db2c0e36","size":131983}%

#��

# �ύһ���洢�������ʱ��weed��Ҫ����һ��ȫ�ֵ��ļ�ID
curl -X POST http://192.168.100.2:9333/dir/assign

# �洢һ��ͼƬ
curl -X PUT -F file=@/tmp/1.jpg http://192.168.100.2:8080/6,04f00144db
```
���� 192.168.100.2:8080/6,61db2c0e36 ���鿴�ϴ��ļ�

----
�ο�:��https://yanyiwu.com/work/2015/01/09/weed-fs-source-analysis.html

