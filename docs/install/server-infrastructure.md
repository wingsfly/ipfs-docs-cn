---
title: 服务器基础设施
description: IPFS Cluster provides data orchestration across a swarm of IPFS daemons by allocating, replicating, and tracking a global pin-set distributed among multiple peers. Learn how to install it here.
---

# 服务器基础设施

如果准备在服务器中安装IPFS，并将IPFS作为服务对外提供，应当安装IPFS集群。通过分配、复制和追踪在多个节点间分布的全局固定数据集，IPFS集群在一个IPFS守护集群中提供了数据协调服务。这显著简化了对多个IPFS节点的管理和确保内网中的数据可用性。

## 创建本地集群

要确认IPFS集群是否适合当前的项目，可以按照此快速入门指南启动一个本地IPFS集群实例。完成该指南后，你将对IPFS集群的设置方法和交互方式有一个深入的理解。如果你要创建一个生产环境可用的集群，参照[官方IPFS集群文档 →](https://cluster.ipfs.io/)

### 先决条件

需要已安装[Docker](https://docs.docker.com/install/)和[Docker Compose](https://docs.docker.com/compose/install/)。通过查询相应版本，确认已正确安装：

```bash
docker version

> Client: Docker Engine - Community
> Version:           19.03.13
> API version:       1.40
> ...

docker-compose version

> docker-compose version 1.27.4, build 40524192
> docker-py version: 4.3.1
> ...
```

如果遇到问题，请前往[官方Docker文档 →](https://docs.docker.com/)以解决问题。

### 步骤

1. 从[dist.ipfs.io](https://dist.ipfs.io/#ipfs-cluster-ctl)下载最新的`ipfs-cluster-ctl`包：

    ```bash
    wget https://dist.ipfs.io/ipfs-cluster-ctl/v0.13.1/ipfs-cluster-ctl_v0.13.1_linux-amd64.tar.gz
    ```

1. 解压包：

   ```bash
   tar xvzf ipfs-cluster-ctl_v0.13.0_linux-amd64.tar.gz

   > ipfs-cluster-ctl/ipfs-cluster-ctl
   > ipfs-cluster-ctl/LICENSE
   > ipfs-cluster-ctl/LICENSE-APACHE
   > ipfs-cluster-ctl/LICENSE-MIT
   > ipfs-cluster-ctl/README.md
   ```

1. 下载[`docker-composer.yml` file](https://raw.githubusercontent.com/ipfs/ipfs-cluster/master/docker-compose.yml)，放在`ipfs-cluster-ctl`目录下：

   ```bash
   wget https://raw.githubusercontent.com/ipfs/ipfs-cluster/master/docker-compose.yml
   ```

1. 使用`docker-compose`启动集群，需要以root身份运行：

   ```bash
   docker-compose up

   > Recreating ipfs2 ... done
   > Recreating ipfs1    ... done
   > Recreating ipfs0    ... done
   > Recreating cluster2 ... done
   > ...
   ```

   可能看到如下的错误：

   ```bash
   cluster2    | 2020-10-27T15:20:15.116Z  ERROR   ipfshttp    error posting to IPFS:Post "http://172.18.0.2:5001/api/v0/pin/ls?type=recursive": dial tcp 172.18.0.2:5001: connect: connection refused
   ```

   当前可以忽略这些错误，这些错误是因为集群中的部分IPFS节点还没有启动，在若干分钟后，所有组件应该都已运行起来：

   ```bash
   > ipfs1       | API server listening on /ip4/0.0.0.0/tcp/5001
   > ipfs1       | WebUI: http://0.0.0.0:5001/webui
   > ipfs1       | Gateway (readonly) server listening on /ip4/0.0.0.0/tcp/8080
   > ipfs1       | Daemon is ready
   ```

1. 现在可以和集群进行交互了。在一个新的终端控制台中，进入`ipfs-cluster-ctl`目录，查看集群的节点：

    ```bash
    ./ipfs-cluster-ctl peers ls

    > 12D3KooWBaQ9SGtdnJmyS2fe7uq5gXQnejCf5ma2n9cUEbwVD5gf | cluster2 | Sees 2 other peers
    > > Addresses:
    > - /ip4/127.0.0.1/tcp/9096/p2p/12D3KooWBaQ9SGtdnJmyS2fe7uq5gXQnejCf5ma2n9cUEbwVD5gf
    > - /ip4/172.18.0.5/tcp/9096/p2p/12D3KooWBaQ9SGtdnJmyS2fe7uq5gXQnejCf5ma2n9cUEbwVD5gf
    > ...
    > 12D3KooWDmjW55h3vSqLmSm1fBxPzs1dHkbtjWSHEj7RhzpcY9vE | cluster0 | Sees 2 other peers
    > > Addresses:
    > - /ip4/127.0.0.1/tcp/9096/p2p/12D3KooWDmjW55h3vSqLmSm1fBxPzs1dHkbtjWSHEj7RhzpcY9vE
    > - /ip4/172.18.0.7/tcp/9096/p2p/12D3KooWDmjW55h3vSqLmSm1fBxPzs1dHkbtjWSHEj7RhzpcY9vE
    > ...
    > 12D3KooWLhGJaddVKj8gsYXfYpyMKL5NhcmtiakDCWhDGtZJSu2w | cluster1 | Sees 2 other peers
    > > Addresses:
    > - /ip4/127.0.0.1/tcp/9096/p2p/12D3KooWLhGJaddVKj8gsYXfYpyMKL5NhcmtiakDCWhDGtZJSu2w
    > - /ip4/172.18.0.6/tcp/9096/p2p/12D3KooWLhGJaddVKj8gsYXfYpyMKL5NhcmtiakDCWhDGtZJSu2w
    > ...
    ```

1. 在集群中添加一个文件：

   ```bash
   wget https://upload.wikimedia.org/wikipedia/commons/6/63/Neptune_-_Voyager_2_%2829347980845%29_flatten_crop.jpg
   ./ipfs-cluster-ctl add Neptune_-_Voyager_2_\(29347980845\)_flatten_crop.jpg

   > added QmdzvHZt6QRJzySuVzURUvKCUzrgGwksvrsnqTryqxD4yn Neptune_-_Voyager_2_(29347980845)_flatten_crop.jpg
   ```

1. 使用以上给出的CID查看IPFS集群中的文件状态：

   ```bash
   ./ipfs-cluster-ctl status QmdzvHZt6QRJzySuVzURUvKCUzrgGwksvrsnqTryqxD4yn

   > QmdzvHZt6QRJzySuVzURUvKCUzrgGwksvrsnqTryqxD4yn:
   > > cluster2             : PINNED | 2020-10-27T15:42:39.984850961Z
   > > cluster0             : PINNED | 2020-10-27T15:42:39.984556496Z
   > > cluster1             : PINNED | 2020-10-27T15:42:39.984842325Z
   ```

   以上显示`QmdzvHZ...`已经在我们集群中的三个IPFS节点中固定了。

1. 当已经完成体验后，可以终止该集群。此操作需要root权限：

   ```bash
   docker-compose kill

   > Killing cluster0 ... done
   > Killing cluster1 ... done
   > Killing cluster2 ... done
   > Killing ipfs1    ... done
   > Killing ipfs0    ... done
   > Killing ipfs2    ... done
   ```

   运行`ipfs-cluster-ctl`守护进程的终端会断开所有当前的连接：

   ```bash
   > ...
   > ipfs0 exited with code 137
   > ipfs1 exited with code 137
   > cluster0 exited with code 137
   > cluster2 exited with code 137
   > cluster1 exited with code 137
   > ipfs2 exited with code 137
   ```

## 下一步

如果需要深入研究IPFS集群，查看该项目的文档[cluster.ipfs.io →](https://cluster.ipfs.io/)
