# CKS-note

Cilium Network policy plugin
AppArmor Plugin in late

Look in Pod without Shell or bash :
```shell
ps aux | grep kube-apiser
root        1549  5.0  7.3 1038116 295892 ?      Ssl  09:17   1:51 kube-apiserver

ls /proc/1549/root/
bin  boot  dev  etc  go-runner  home  lib  proc  root  run  sbin  sys  tmp  usr  var

find /proc/1549/root/ | grep kube-api
/proc/1549/root/usr/local/bin/kube-apiserver

sha256sum /proc/1549/root/usr/local/bin/kube-apiserver > compare
```


Add a user context :
```shell
# Create cert
openssl genrsa -out jim.key 2048
openssl req -new -key jim.key -out jim.csr

create certif with base64 csr


kubectl config set-credentials jim --client-key=./jim.key --client-certificate=./jim.crt --embed-certs
kubectl set-context jim --user=jim --cluster=kubernetes
```


#Limit linux access :
```shell
usermod -s /bin/nologin micahel
grep -i michael /etc/passwd
    michale:x:1001:1001::/home/michael:/bin/nologin

userdel bob
grep -i bob /etc/passwd
    

apt list --installed

systemctl list-units --type service
rm /lib/systemd/system/nginx.service

lsmod
/etc/modprobe.d/blacklist.conf

nestat -natp | grep PORT_NUMBER
cat /etc/service | grep PORT_NUMBER ???

ufw allow from 135.22.65.0/24 to any port 9090 proto tcp
ufw deny from any to 127.0.0.1 port 80
```

# SYSCALL

```shell
strace -c ls /root

grep -w 335 /usr/include/asm/unistd_64.h # >> list syscall nb to syscall name

#Tracee container
sudo docker run --name tracee --rm --privileged --pid=host \
-v /lib/modules/:/lib/modules/:ro -v /usr/src:/usr/src:ro \
-v /tmp/tracee:/tmp/tracee aquasec/tracee:0.4.0 --trace container=new
```

Seccomp profile location by default is set to /var/lib/kubelet/seccomp

## AppArmor 
```shell
cd /etc/apparmor.d/
apparmor_parser -r /etc/apparmor.d/profile.name
docker run --rm -it --security-opt apparmor=docker-default hello-world
```

## Seccomp
```shell
docker run -it --rm --security-opt seccomp=unconfined docker/whalessay /bin/sh
```


# Capabilities

```shell
getcap /usr/bin/ping

getpcaps 779 # 779 is thePID of a process

```

# Admission Controller

```shell
kube-apiserver -h |grep enable-admission-plugins
# For kubeAdm system :
kubectl exec kube-apiserver-controlplane -n kube-system -- kube-apiserver -h |grep enable-admission-plugins
```

In `kube-apiserver.yaml` flag ``--enable-admission-plugins``

# OPA 

```shell
#load rule
curl -X PUT --data-binary @file.rego http://localhost:8181/v1/policies/policyname
```

# SandBoxing


## Kvisor

handler: runsc
```shell

apiVeresion: node.k8s.io/v1beta1
kind: RuntimeClass
metadata:
    name: gvisor
handler: runsc

k create -f 

kind: Pod
spec:
    runtimeClassName: gvisor
```
rubn


## Kata
    handler : kata

# mTls

## Istio
## Linkerd


# Supply Chain Security

## Static analysis / Kubesec

```
    kubesec scan /file/to/scan >> report.scan
```

## CVE Identification / Trivy

```
trivy image --severity CRITICAL nginx:1.18.0
trivy image --severity CRITICAL,HIGH nginx:1.18.0
trivy image --ignore-unfixed nginx:1.18.0

docker save nginx:1.18.0 > nginx.tar
trivy image --input archive.tar
```

# Monitoring Logging & Runtive Security

## Perform behavioral analytics of syscall process
