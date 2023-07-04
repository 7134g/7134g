### 部署
- Windows
  `https://github.com/etcd-io/etcd`
- Linux or MacOS
  `docker pull bitnami/etcd`
  `docker run -it --name Etcd bitnami/etcd`


### 连接服务
`etcdctl --endpoints=http://192.168.1.13:2379 endpoint status`
