# etcd

A highly-available key value store for shared configuration and service discovery.

用于配置共享和服务发现的键值存储系统，其四个核心特点：

简单：基于HTTP+JSON的API让你用curl命令就可以轻松使用。

安全：可选SSL客户认证机制。

快速：每个实例每秒支持一千次写操作。

可信：使用Raft算法充分实现了分布式。


## docker 官方推荐镜像

etcd uses gcr.io/etcd-development/etcd as a primary container registry, 

and quay.io/coreos/etcd as secondary.

- [gcr.io/etcd-development/etcd](https://gcr.io/etcd-development/etcd)
- [quay.io/coreos/etcd](https://quay.io/coreos/etcd)

## 示例

```
rm -rf /tmp/etcd-data.tmp && mkdir -p /tmp/etcd-data.tmp && \
  docker rmi gcr.io/etcd-development/etcd:v3.4.3 || true && \
  docker run \
  -p 2379:2379 \
  -p 2380:2380 \
  --mount type=bind,source=/tmp/etcd-data.tmp,destination=/etcd-data \
  --name etcd-gcr-v3.4.3 \
  gcr.io/etcd-development/etcd:v3.4.3 \
  /usr/local/bin/etcd \
  --name s1 \
  --data-dir /etcd-data \
  --listen-client-urls http://0.0.0.0:2379 \
  --advertise-client-urls http://0.0.0.0:2379 \
  --listen-peer-urls http://0.0.0.0:2380 \
  --initial-advertise-peer-urls http://0.0.0.0:2380 \
  --initial-cluster s1=http://0.0.0.0:2380 \
  --initial-cluster-token tkn \
  --initial-cluster-state new \
  --log-level info \
  --logger zap \
  --log-outputs stderr

docker exec etcd-gcr-v3.4.3 /bin/sh -c "/usr/local/bin/etcd --version"
docker exec etcd-gcr-v3.4.3 /bin/sh -c "/usr/local/bin/etcdctl version"
docker exec etcd-gcr-v3.4.3 /bin/sh -c "/usr/local/bin/etcdctl endpoint health"
docker exec etcd-gcr-v3.4.3 /bin/sh -c "/usr/local/bin/etcdctl put foo bar"
docker exec etcd-gcr-v3.4.3 /bin/sh -c "/usr/local/bin/etcdctl get foo"
```
## ETCD 参数说明
- data-dir: 指定节点的数据存储目录，这些数据包括节点ID，集群ID，集群初始化配置，Snapshot文件，若未指定—wal-dir，还会存储WAL文件；
- wal-dir: 指定节点的was文件的存储目录，若指定了该参数，wal文件会和其他数据文件分开存储。
- name: 节点名称
- initial-advertise-peer-urls: 告知集群其他节点url.(对于集群内提供服务的url)
- listen-peer-urls: 监听URL，用于与其他节点通讯
- advertise-client-urls: 告知客户端url, 也就是服务的url(对外提供服务的utl)
- initial-cluster-token: 集群的ID
- initial-cluster: 集群中所有节点


## 参考

- [etcd 官网](https://etcd.io/)
- [etcd docker 集群](https://github.com/retr0h/compose-etcd)
- [etcd release page](https://github.com/etcd-io/etcd/releases)
- [etcd在docker中使用](https://blog.csdn.net/ucmir183/article/details/84454575)
- [ETCD 与 服务发现](https://www.jianshu.com/p/3bd041807974)