# Kubernetes Redis with High Availability

Redis Images and Samples in this project are implemented using the latest features of Kubernetes:

* StatefulSet
* Init Container

# Requirements

* Kubernetes 1.8 cluster (for 1.6 use original  tarosky/k8s-redis-ha)
* Redis 3.2

# Quick Start

If you already have a Kubernetes cluster, you can deploy High Availability Redis using the following command:

```console
$ kubectl create -f example/
service "redis-sentinel" created
statefulset "redis-sentinel" created
service "redis-server" created
statefulset "redis-server" created
pod "console" created
```

## Accessing Redis

You can access Redis server using `console` pod:

```console
$ kubectl exec -ti console -- /bin/bash
root@console:# export MASTER_IP="$(redis-cli -h redis-sentinel -p 26379 sentinel get-master-addr-by-name mymaster | head -1)"
root@console:# export SERVER_PASS="_redis-server._tcp.redis-server.default.svc.cluster.local"
root@console:# redis-cli -h "${MASTER_IP}" -a "${SERVER_PASS}" set foo bar
OK
root@console:# redis-cli -h redis-server -a "${SERVER_PASS}" get foo
bar
root@console:# redis-cli -h redis-server -a "${SERVER_PASS}" KEYS *
....
```

The passowrd is necessary for preventing independent Redis clusters from merging together in some circumstances.

## Scale Up and Down

With `tarosky/k8s-redis-ha`, you can scale up/down Redis servers and Redis sentinels like the normal Deployment resources:

```console
$ kubectl scale --replicas=5 statefulset/redis-sentinel
statefulset "redis-sentinel" scaled
$ kubectl scale --replicas=5 statefulset/redis-server
statefulset "redis-server" scaled
```

After these scale up/down, the expected number of available slaves in the Redis set are reset automatically.

Deploy Express-Gateway using express-gateway.yml
example uses nfs volume, change as needed

configure EG CLI to use service IP address(Node's IP) and port 30001
```yml

cli:
  url: http://10.0.0.68:31573
```
use CLI as needed. 
