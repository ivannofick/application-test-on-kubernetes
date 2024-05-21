# application-test-on-kubernetes
repository for learning kubernetes using k3s on mac m1
- run on localy laptop
-- nodemon index.js(if you have nodemon, if u havent nodemon u can use node )

## 1. Checking K3s on mac m1 (local)
```
$ curl -sfL https://get.k3s.io | sh -

[ERROR]Can not find systemd or openrc to use as a process supervisor for k3s

```

## 2. Installing multipass
```
$ brew install --cask multipass
```

## 3. Create New VM with specifying memory and disk space
```
$ multipass launch --name k3s --mem 4G --disk 40G
```

## 4. Once VM is launched, see VM Detail
```
$ multipass info k3s
Name: k3s
State: Running
IPv4: 192.168.64.6
                10.42.0.0
                10.42.0.1
Release: Ubuntu 22.04.1 LTS
Image hash: 78b5ca0da456 (Ubuntu 22.04 LTS)
Load: 1.34 2.10 1.70
Disk usage: 3.7G out of 38.6G
Memory usage: 1.2G out of 3.8G
Mounts: /Users/chillaranand/test/k8s => ~/k8s
                    UID map: 503:default
                    GID map: 20:default
```

## 5. install k3s inside VM
```

$ multipass shell k3s
ubuntu@k3s:~$ curl -sfL https://get.k3s.io | sh -

```

## 6. Get token and ip of VM k3s
```
$ multipass exec k3s sudo cat /var/lib/rancher/k3s/server/node-token
K109b0c704e12c712b1470f5f121ee8c6339e03b445fe0e1840427bee628744aaa6::server:86eb8217591da9aa4a4b5278e5505176

$ multipass info k3s | grep -i ip
IPv4:           192.168.64.6

```
## 7. Create VM for cluster and c
```
$ multipass launch --name k3s-worker --mem 2G --disk 20G
$ multipass shell k3s-worker

ubuntu@k3s-worker:~$ curl -sfL https://get.k3s.io | K3S_URL=https://{{ using IP onpoint 6}}:6443 K3S_TOKEN="{{ using get token point 6}}" sh -

example 
ubuntu@k3s-worker:~$ curl -sfL https://get.k3s.io | K3S_URL=https://192.168.64.6:6443 K3S_TOKEN="K109b0c704e12c712b1470f5f121ee8c6339e03b445fe0e1840427bee628744aaa6::server:86eb8217591da9aa4a4b5278e5505176" sh -

```
## 8. Verify if the node is addded correctly from k3s VM

```
$ multipass shell k3s
ubuntu@k3s:~$ kubectl get nodes
NAME         STATUS   ROLES                  AGE    VERSION
k3s-worker   Ready    <none>                 81m    v1.29.4+k3s1
k3s          Ready    control-plane,master   117m   v1.29.4+k3s1
```

## 9. Check list on multipass for watch IP VM
```
$ multipass list
```






# Delete VM on multipass
```
$ multipass delete k3s k3s-worker

$ multipass list                 
Name                    State             IPv4             Image
k3s                     Deleted           --               Ubuntu 24.04 LTS
k3s-worker              Deleted           --               Ubuntu 24.04 LTS


$ multipass purge

$ multipass list
No instances found.
```