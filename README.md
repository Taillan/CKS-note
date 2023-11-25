# CKS-note

Cilium Network policy plugin
AppArmor Plugin in late

Look in Pod without Shell or bash :
```
ps aux | grep kube-apiser
root        1549  5.0  7.3 1038116 295892 ?      Ssl  09:17   1:51 kube-apiserver

ls /proc/1549/root/
bin  boot  dev  etc  go-runner  home  lib  proc  root  run  sbin  sys  tmp  usr  var

find /proc/1549/root/ | grep kube-api
/proc/1549/root/usr/local/bin/kube-apiserver

sha256sum /proc/1549/root/usr/local/bin/kube-apiserver > compare
```


Add a user context :
```
# Create cert
openssl genrsa -out jim.key 2048
openssl req -new -key jim.key -out jim.csr

kubectl config set-credentials jim --client-key=./jim.key --client-certificate=./jim.crt --embed-certs
kubectl set-context jim --user=jim --cluster=kubernetes
```