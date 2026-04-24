minikube@minikube-virtual-machine:~/serviceAccount$ mkdir TP
minikube@minikube-virtual-machine:~/serviceAccount$ cd TP
minikube@minikube-virtual-machine:~/serviceAccount/TP$ gedit pod-default.yaml
minikube@minikube-virtual-machine:~/serviceAccount/TP$ kubectl apply -f pod-default.yaml
[sudo] password for minikube: 
pod/pod-default created
minikube@minikube-virtual-machine:~/serviceAccount/TP$ kubectl get po
NAME            READY   STATUS             RESTARTS        AGE
alpine          0/1     CrashLoopBackOff   8 (4m30s ago)   3d3h
monitoring      1/1     Running            0               164m
pod-default     1/1     Running            0               10s
private-image   0/1     ImagePullBackOff   0               2d21h
proxy           1/1     Running            1 (28h ago)     43h
minikube@minikube-virtual-machine:~/serviceAccount/TP$ kubectl exec -ti pod-default -- sh
/ # apk add --update curl
fetch https://dl-cdn.alpinelinux.org/alpine/v3.15/main/x86_64/APKINDEX.tar.gz
fetch https://dl-cdn.alpinelinux.org/alpine/v3.15/community/x86_64/APKINDEX.tar.gz
(1/5) Installing ca-certificates (20230506-r0)
(2/5) Installing brotli-libs (1.0.9-r5)
(3/5) Installing nghttp2-libs (1.46.0-r2)
(4/5) Installing libcurl (8.5.0-r0)
(5/5) Installing curl (8.5.0-r0)
Executing busybox-1.34.1-r7.trigger
Executing ca-certificates-20230506-r0.trigger
OK: 8 MiB in 19 packages
/ # curl https://kubernetes/api/v1 --insecure
{
  "kind": "Status",
  "apiVersion": "v1",
  "metadata": {},
  "status": "Failure",
  "message": "Unauthorized",
  "reason": "Unauthorized",
  "code": 401
}/ # •Résultat :
sh: •Résultat: not found
/ # curl https://kubernetes/api/v1 --insecure
{
  "kind": "Status",
  "apiVersion": "v1",
  "metadata": {},
  "status": "Failure",
  "message": "Unauthorized",
  "reason": "Unauthorized",
  "code": 401
}/ # ls /run/secrets/kubernetes.io/serviceaccount/
ca.crt     namespace  token
/ # cat /run/secrets/kubernetes.io/serviceaccount/token
eyJhbGciOiJSUzI1NiIsImtpZCI6IkNCSHdaRUFQUzdoNlRGRGlZY1VRcmFhZVp2OWQ2NUZpY04wamFCdzlheGcifQ.eyJhdWQiOlsiaHR0cHM6Ly9rdWJlcm5ldGVzLmRlZmF1bHQuc3ZjIl0sImV4cCI6MTgwODQ3OTg5NiwiaWF0IjoxNzc2OTQzODk2LCJpc3MiOiJodHRwczovL2t1YmVybmV0ZXMuZGVmYXVsdC5zdmMiLCJqdGkiOiJhZDg5ZDkwYy02OWY0LTQ1ZDgtYjAyMy05MzNhNzE1MmNlYzMiLCJrdWJlcm5ldGVzLmlvIjp7Im5hbWVzcGFjZSI6ImRlZmF1bHQiLCJub2RlIjp7Im5hbWUiOiJtaW5pa3ViZS12aXJ0dWFsLW1hY2hpbmUiLCJ1aWQiOiJjMWEzZjU1NS1mOWU0LTRmMTUtYjhhOS01Yjg4NmNhMTRhYzgifSwicG9kIjp7Im5hbWUiOiJwb2QtZGVmYXVsdCIsInVpZCI6IjkyMzAyN2RmLTY5NGYtNGM0MS05MGJiLTliMGY5MGNiZjAxNiJ9LCJzZXJ2aWNlYWNjb3VudCI6eyJuYW1lIjoiZGVmYXVsdCIsInVpZCI6IjU4NmZjYjlhLTIwYWUtNDg4ZS1hY2I2LTZkNmVlNDc3ODU0NCJ9LCJ3YXJuYWZ0ZXIiOjE3NzY5NDc1MDN9LCJuYmYiOjE3NzY5NDM4OTYsInN1YiI6InN5c3RlbTpzZXJ2aWNlYWNjb3VudDpkZWZhdWx0OmRlZmF1bHQifQ.hyR72GtjDokKbWTusa0SrS_WLQMyf_NwTUzAH0wqL11LTb3yesHn9cnXB7nPq5JDfycVZB02sS0OMwqMMRasuHNZgwMq-jBaT48hxEf8QcnJqOrM9e7oLqx90Oo7Tv-_9rq8TXS1GBIMqpXNX3OFe32ZWMpff_SmSBN7MtGv2ySs79LpRywehBnzTEM46X1ExdOWgwvWOZN3_dK7_GTWKwm_5apt9S4RvH5r-E0hNcVckQpaxhEeONC9Wb6IuWc6CSOXsa2jL9gIvw03z0F7wjhmAhVt11WbSlLjtgFVufH6D2U91b_DG7Q5AK88I0nwBwzmUpIJ3qrE6bv14wCY2g/ # exit
minikube@minikube-virtual-machine:~/serviceAccount/TP$  kubectl get svc
NAME         TYPE        CLUSTER-IP   EXTERNAL-IP   PORT(S)   AGE
kubernetes   ClusterIP   10.96.0.1    <none>        443/TCP   6d4h
minikube@minikube-virtual-machine:~/serviceAccount/TP$ curl  https://kubernetes/api/v1/namespaces/default/pods/
curl: (6) Could not resolve host: kubernetes
minikube@minikube-virtual-machine:~/serviceAccount/TP$ kubectl get sa --all-namespaces | grep default
default                default                                       6d4h
default                monitoring                                    3h24m
k0s-autopilot          default                                       6d4h
kube-node-lease        default                                       6d4h
kube-public            default                                       6d4h
kube-system            default                                       6d4h
kubernetes-dashboard   default                                       22h
minikube@minikube-virtual-machine:~/serviceAccount/TP$ kubectl create token default
eyJhbGciOiJSUzI1NiIsImtpZCI6IkNCSHdaRUFQUzdoNlRGRGlZY1VRcmFhZVp2OWQ2NUZpY04wamFCdzlheGcifQ.eyJhdWQiOlsiaHR0cHM6Ly9rdWJlcm5ldGVzLmRlZmF1bHQuc3ZjIl0sImV4cCI6MTc3Njk0ODQxMywiaWF0IjoxNzc2OTQ0ODEzLCJpc3MiOiJodHRwczovL2t1YmVybmV0ZXMuZGVmYXVsdC5zdmMiLCJqdGkiOiJjMDJkYmQyMy1iM2M5LTRlZjUtOWFjNy1lN2Y3OTljODhhYTIiLCJrdWJlcm5ldGVzLmlvIjp7Im5hbWVzcGFjZSI6ImRlZmF1bHQiLCJzZXJ2aWNlYWNjb3VudCI6eyJuYW1lIjoiZGVmYXVsdCIsInVpZCI6IjU4NmZjYjlhLTIwYWUtNDg4ZS1hY2I2LTZkNmVlNDc3ODU0NCJ9fSwibmJmIjoxNzc2OTQ0ODEzLCJzdWIiOiJzeXN0ZW06c2VydmljZWFjY291bnQ6ZGVmYXVsdDpkZWZhdWx0In0.Qyx4rYBC7pyn_X-9crOfdHwd6hZMFoZ-4VWkHcd6ctYwCzlXLwdU0lSVDvPTB-DtV6Hi86msnN3E9YgInM2qcpNVG1qiDGxp2bnXHF0RR2kevmSguUHo3T-Z83XUJ0xl8UP2ktzYgI0g224vrMUQTbO7REIl3ADYbY0g95_8fHk4Zn8mWuByN-CcDTIrfQARjdtW6t36mtdQV3DnzpS8c6Qrv7Su-Us_OZm3zjfq_5UHpVOmvJSkvd6zOTboE1HM-FXFcO0vR1NtJyrjQQiFjM3l6YRqLQrhSmNsJb2X_DQwudejW9wX9HG1W3xQfVNnrmeuTOvyKApc3saj765fWQ
minikube@minikube-virtual-machine:~/serviceAccount/TP$ echo "eyJhbGciOiJSUzI1NiIsImtpZCI6IkNCSHdaRUFQUzdoNlRGRGlZY1VRcmFhZVp2OWQ2NUZpY04wamFCdzlheGcifQ.eyJhdWQiOlsiaHR0cHM6Ly9rdWJlcm5ldGVzLmRlZmF1bHQuc3ZjIl0sImV4cCI6MTc3Njk0ODQxMywiaWF0IjoxNzc2OTQ0ODEzLCJpc3MiOiJodHRwczovL2t1YmVybmV0ZXMuZGVmYXVsdC5zdmMiLCJqdGkiOiJjMDJkYmQyMy1iM2M5LTRlZjUtOWFjNy1lN2Y3OTljODhhYTIiLCJrdWJlcm5ldGVzLmlvIjp7Im5hbWVzcGFjZSI6ImRlZmF1bHQiLCJzZXJ2aWNlYWNjb3VudCI6eyJuYW1lIjoiZGVmYXVsdCIsInVpZCI6IjU4NmZjYjlhLTIwYWUtNDg4ZS1hY2I2LTZkNmVlNDc3ODU0NCJ9fSwibmJmIjoxNzc2OTQ0ODEzLCJzdWIiOiJzeXN0ZW06c2VydmljZWFjY291bnQ6ZGVmYXVsdDpkZWZhdWx0In0.Qyx4rYBC7pyn_X-9crOfdHwd6hZMFoZ-4VWkHcd6ctYwCzlXLwdU0lSVDvPTB-DtV6Hi86msnN3E9YgInM2qcpNVG1qiDGxp2bnXHF0RR2kevmSguUHo3T-Z83XUJ0xl8UP2ktzYgI0g224vrMUQTbO7REIl3ADYbY0g95_8fHk4Zn8mWuByN-CcDTIrfQARjdtW6t36mtdQV3DnzpS8c6Qrv7Su-Us_OZm3zjfq_5UHpVOmvJSkvd6zOTboE1HM-FXFcO0vR1NtJyrjQQiFjM3l6YRqLQrhSmNsJb2X_DQwudejW9wX9HG1W3xQfVNnrmeuTOvyKApc3saj765fWQ" | cut -d '.' -f2 | base64 -d | jq
base64: invalid input
Command 'jq' not found, but can be installed with:
sudo snap install jq  # version 1.5+dfsg-1, or
sudo apt  install jq  # version 1.6-2.1ubuntu3.1
See 'snap info jq' for additional versions.
minikube@minikube-virtual-machine:~/serviceAccount/TP$ sudo apt upadate && sudo apt  install jq
E: Invalid operation upadate
minikube@minikube-virtual-machine:~/serviceAccount/TP$ sudo apt update && sudo apt  install jq
Hit:1 http://fr.archive.ubuntu.com/ubuntu jammy InRelease
Get:2 http://security.ubuntu.com/ubuntu jammy-security InRelease [129 kB]
Get:3 http://fr.archive.ubuntu.com/ubuntu jammy-updates InRelease [128 kB]
Hit:4 http://fr.archive.ubuntu.com/ubuntu jammy-backports InRelease
Get:5 http://security.ubuntu.com/ubuntu jammy-security/main amd64 Packages [3 171 kB]
Get:6 http://security.ubuntu.com/ubuntu jammy-security/main i386 Packages [812 kB]
Get:7 http://security.ubuntu.com/ubuntu jammy-security/main amd64 c-n-f Metadata [14,2 kB]
Get:8 http://fr.archive.ubuntu.com/ubuntu jammy-updates/main amd64 Packages [3 436 kB]
Get:9 http://fr.archive.ubuntu.com/ubuntu jammy-updates/main i386 Packages [996 kB]
Get:10 http://fr.archive.ubuntu.com/ubuntu jammy-updates/universe i386 Packages [806 kB]
Get:11 http://fr.archive.ubuntu.com/ubuntu jammy-updates/universe amd64 Packages [1 268 kB]
Fetched 10,8 MB in 7s (1 589 kB/s)                                                               
Reading package lists... Done
Building dependency tree... Done
Reading state information... Done
5 packages can be upgraded. Run 'apt list --upgradable' to see them.
Reading package lists... Done
Building dependency tree... Done
Reading state information... Done
The following additional packages will be installed:
  libjq1 libonig5
The following NEW packages will be installed:
  jq libjq1 libonig5
0 upgraded, 3 newly installed, 0 to remove and 5 not upgraded.
Need to get 359 kB of archives.
After this operation, 1 087 kB of additional disk space will be used.
Do you want to continue? [Y/n] Y
Get:1 http://fr.archive.ubuntu.com/ubuntu jammy/main amd64 libonig5 amd64 6.9.7.1-2build1 [172 kB]
Get:2 http://fr.archive.ubuntu.com/ubuntu jammy-updates/main amd64 libjq1 amd64 1.6-2.1ubuntu3.2 [135 kB]
Get:3 http://fr.archive.ubuntu.com/ubuntu jammy-updates/main amd64 jq amd64 1.6-2.1ubuntu3.2 [52,5 kB]
Fetched 359 kB in 0s (2 945 kB/s)
Selecting previously unselected package libonig5:amd64.
(Reading database ... 209319 files and directories currently installed.)
Preparing to unpack .../libonig5_6.9.7.1-2build1_amd64.deb ...
Unpacking libonig5:amd64 (6.9.7.1-2build1) ...
Selecting previously unselected package libjq1:amd64.
Preparing to unpack .../libjq1_1.6-2.1ubuntu3.2_amd64.deb ...
Unpacking libjq1:amd64 (1.6-2.1ubuntu3.2) ...
Selecting previously unselected package jq.
Preparing to unpack .../jq_1.6-2.1ubuntu3.2_amd64.deb ...
Unpacking jq (1.6-2.1ubuntu3.2) ...
Setting up libonig5:amd64 (6.9.7.1-2build1) ...
Setting up libjq1:amd64 (1.6-2.1ubuntu3.2) ...
Setting up jq (1.6-2.1ubuntu3.2) ...
Processing triggers for man-db (2.10.2-1) ...
Processing triggers for libc-bin (2.35-0ubuntu3.13) ...
minikube@minikube-virtual-machine:~/serviceAccount/TP$ echo "eyJhbGciOiJSUzI1NiIsImtpZCI6IkNCSHdaRUFQUzdoNlRGRGlZY1VRcmFhZVp2OWQ2NUZpY04wamFCdzlheGcifQ.eyJhdWQiOlsiaHR0cHM6Ly9rdWJlcm5ldGVzLmRlZmF1bHQuc3ZjIl0sImV4cCI6MTc3Njk0ODQxMywiaWF0IjoxNzc2OTQ0ODEzLCJpc3MiOiJodHRwczovL2t1YmVybmV0ZXMuZGVmYX/ # TOKEN=$(cat /run/secrets/kubernetes.io/serviceaccount/token)
/ # cat $TOKEN
cat: can't open 'eyJhbGciOiJSUzI1NiIsImtpZCI6IkNCSHdaRUFQUzdoNlRGRGlZY1VRcmFhZVp2OWQ2NUZpY04wamFCdzlheGcifQ.eyJhdWQiOlsiaHR0cHM6Ly9rdWJlcm5ldGVzLmRlZmF1bHQuc3ZjIl0sImV4cCI6MTgwODQ3OTg5NiwiaWF0IjoxNzc2OTQzODk2LCJpc3MiOiJodHRwczovL2t1YmVybmV0ZXMuZGVmYXVsdC5zdmMiLCJqdGkiOiJhZDg5ZDkwYy02OWY0LTQ1ZDgtYjAyMy05MzNhNzE1MmNlYzMiLCJrdWJlcm5ldGVzLmlvIjp7Im5hbWVzcGFjZSI6ImRlZmF1bHQiLCJub2RlIjp7Im5hbWUiOiJtaW5pa3ViZS12aXJ0dWFsLW1hY2hpbmUiLCJ1aWQiOiJjMWEzZjU1NS1mOWU0LTRmMTUtYjhhOS01Yjg4NmNhMTRhYzgifSwicG9kIjp7Im5hbWUiOiJwb2QtZGVmYXVsdCIsInVpZCI6IjkyMzAyN2RmLTY5NGYtNGM0MS05MGJiLTliMGY5MGNiZjAxNiJ9LCJzZXJ2aWNlYWNjb3VudCI6eyJuYW1lIjoiZGVmYXVsdCIsInVpZCI6IjU4NmZjYjlhLTIwYWUtNDg4ZS1hY2I2LTZkNmVlNDc3ODU0NCJ9LCJ3YXJuYWZ0ZXIiOjE3NzY5NDc1MDN9LCJuYmYiOjE3NzY5NDM4OTYsInN1YiI6InN5c3RlbTpzZXJ2aWNlYWNjb3VudDpkZWZhdWx0OmRlZmF1bHQifQ.hyR72GtjDokKbWTusa0SrS_WLQMyf_NwTUzAH0wqL11LTb3yesHn9cnXB7nPq5JDfycVZB02sS0OMwqMMRasuHNZgwMq-jBaT48hxEf8QcnJqOrM9e7oLqx90Oo7Tv-_9rq8TXS1GBIMqpXNX3OFe32ZWMpff_SmSBN7MtGv2ySs79LpRywehBnzTEM46X1ExdOWgwvWOZN3_dK7_GTWKwm_5apt9S4RvH5r-E0hNcVckQpaxhEeONC9Wb6IuWc6CSOXsa2jL9gIvw03z0F7wjhmAhVt11WbSlLjtgFVufH6D2U91b_DG7Q5AK88I0nwBwzmUpIJ3qrE6bv14wCY2g': Filename too long
/ # curl -H "Authorization: Bearer $TOKEN" \
> https://kubernetes/api/v1 --insecure
{
  "kind": "APIResourceList",
  "groupVersion": "v1",
  "resources": [
    {
      "name": "bindings",
      "singularName": "binding",
      "namespaced": true,
      "kind": "Binding",
      "verbs": [
        "create"
      ]
    },
    {
      "name": "componentstatuses",
      "singularName": "componentstatus",
      "namespaced": false,
      "kind": "ComponentStatus",
      "verbs": [
        "get",
        "list"
      ],
      "shortNames": [
        "cs"
      ]
    },
    {
      "name": "configmaps",
      "singularName": "configmap",
      "namespaced": true,
      "kind": "ConfigMap",
      "verbs": [
        "create",
        "delete",
        "deletecollection",
        "get",
        "list",
        "patch",
        "update",
        "watch"
      ],
      "shortNames": [
        "cm"
      ],
      "storageVersionHash": "qFsyl6wFWjQ="
    },
    {
      "name": "endpoints",
      "singularName": "endpoints",
      "namespaced": true,
      "kind": "Endpoints",
      "verbs": [
        "create",
        "delete",
        "deletecollection",
        "get",
        "list",
        "patch",
        "update",
        "watch"
      ],
      "shortNames": [
        "ep"
      ],
      "storageVersionHash": "fWeeMqaN/OA="
    },
    {
      "name": "events",
      "singularName": "event",
      "namespaced": true,
      "kind": "Event",
      "verbs": [
        "create",
        "delete",
        "deletecollection",
        "get",
        "list",
        "patch",
        "update",
        "watch"
      ],
      "shortNames": [
        "ev"
      ],
      "storageVersionHash": "r2yiGXH7wu8="
    },
    {
      "name": "limitranges",
      "singularName": "limitrange",
      "namespaced": true,
      "kind": "LimitRange",
      "verbs": [
        "create",
        "delete",
        "deletecollection",
        "get",
        "list",
        "patch",
        "update",
        "watch"
      ],
      "shortNames": [
        "limits"
      ],
      "storageVersionHash": "EBKMFVe6cwo="
    },
    {
      "name": "namespaces",
      "singularName": "namespace",
      "namespaced": false,
      "kind": "Namespace",
      "verbs": [
        "create",
        "delete",
        "get",
        "list",
        "patch",
        "update",
        "watch"
      ],
      "shortNames": [
        "ns"
      ],
      "storageVersionHash": "Q3oi5N2YM8M="
    },
    {
      "name": "namespaces/finalize",
      "singularName": "",
      "namespaced": false,
      "kind": "Namespace",
      "verbs": [
        "update"
      ]
    },
    {
      "name": "namespaces/status",
      "singularName": "",
      "namespaced": false,
      "kind": "Namespace",
      "verbs": [
        "get",
        "patch",
        "update"
      ]
    },
    {
      "name": "nodes",
      "singularName": "node",
      "namespaced": false,
      "kind": "Node",
      "verbs": [
        "create",
        "delete",
        "deletecollection",
        "get",
        "list",
        "patch",
        "update",
        "watch"
      ],
      "shortNames": [
        "no"
      ],
      "storageVersionHash": "XwShjMxG9Fs="
    },
    {
      "name": "nodes/proxy",
      "singularName": "",
      "namespaced": false,
      "kind": "NodeProxyOptions",
      "verbs": [
        "create",
        "delete",
        "get",
        "patch",
        "update"
      ]
    },
    {
      "name": "nodes/status",
      "singularName": "",
      "namespaced": false,
      "kind": "Node",
      "verbs": [
        "get",
        "patch",
        "update"
      ]
    },
    {
      "name": "persistentvolumeclaims",
      "singularName": "persistentvolumeclaim",
      "namespaced": true,
      "kind": "PersistentVolumeClaim",
      "verbs": [
        "create",
        "delete",
        "deletecollection",
        "get",
        "list",
        "patch",
        "update",
        "watch"
      ],
      "shortNames": [
        "pvc"
      ],
      "storageVersionHash": "QWTyNDq0dC4="
    },
    {
      "name": "persistentvolumeclaims/status",
      "singularName": "",
      "namespaced": true,
      "kind": "PersistentVolumeClaim",
      "verbs": [
        "get",
        "patch",
        "update"
      ]
    },
    {
      "name": "persistentvolumes",
      "singularName": "persistentvolume",
      "namespaced": false,
      "kind": "PersistentVolume",
      "verbs": [
        "create",
        "delete",
        "deletecollection",
        "get",
        "list",
        "patch",
        "update",
        "watch"
      ],
      "shortNames": [
        "pv"
      ],
      "storageVersionHash": "HN/zwEC+JgM="
    },
    {
      "name": "persistentvolumes/status",
      "singularName": "",
      "namespaced": false,
      "kind": "PersistentVolume",
      "verbs": [
        "get",
        "patch",
        "update"
      ]
    },
    {
      "name": "pods",
      "singularName": "pod",
      "namespaced": true,
      "kind": "Pod",
      "verbs": [
        "create",
        "delete",
        "deletecollection",
        "get",
        "list",
        "patch",
        "update",
        "watch"
      ],
      "shortNames": [
        "po"
      ],
      "categories": [
        "all"
      ],
      "storageVersionHash": "xPOwRZ+Yhw8="
    },
    {
      "name": "pods/attach",
      "singularName": "",
      "namespaced": true,
      "kind": "PodAttachOptions",
      "verbs": [
        "create",
        "get"
      ]
    },
    {
      "name": "pods/binding",
      "singularName": "",
      "namespaced": true,
      "kind": "Binding",
      "verbs": [
        "create"
      ]
    },
    {
      "name": "pods/ephemeralcontainers",
      "singularName": "",
      "namespaced": true,
      "kind": "Pod",
      "verbs": [
        "get",
        "patch",
        "update"
      ]
    },
    {
      "name": "pods/eviction",
      "singularName": "",
      "namespaced": true,
      "group": "policy",
      "version": "v1",
      "kind": "Eviction",
      "verbs": [
        "create"
      ]
    },
    {
      "name": "pods/exec",
      "singularName": "",
      "namespaced": true,
      "kind": "PodExecOptions",
      "verbs": [
        "create",
        "get"
      ]
    },
    {
      "name": "pods/log",
      "singularName": "",
      "namespaced": true,
      "kind": "Pod",
      "verbs": [
        "get"
      ]
    },
    {
      "name": "pods/portforward",
      "singularName": "",
      "namespaced": true,
      "kind": "PodPortForwardOptions",
      "verbs": [
        "create",
        "get"
      ]
    },
    {
      "name": "pods/proxy",
      "singularName": "",
      "namespaced": true,
      "kind": "PodProxyOptions",
      "verbs": [
        "create",
        "delete",
        "get",
        "patch",
        "update"
      ]
    },
    {
      "name": "pods/resize",
      "singularName": "",
      "namespaced": true,
      "kind": "Pod",
      "verbs": [
        "get",
        "patch",
        "update"
      ]
    },
    {
      "name": "pods/status",
      "singularName": "",
      "namespaced": true,
      "kind": "Pod",
      "verbs": [
        "get",
        "patch",
        "update"
      ]
    },
    {
      "name": "podtemplates",
      "singularName": "podtemplate",
      "namespaced": true,
      "kind": "PodTemplate",
      "verbs": [
        "create",
        "delete",
        "deletecollection",
        "get",
        "list",
        "patch",
        "update",
        "watch"
      ],
      "storageVersionHash": "LIXB2x4IFpk="
    },
    {
      "name": "replicationcontrollers",
      "singularName": "replicationcontroller",
      "namespaced": true,
      "kind": "ReplicationController",
      "verbs": [
        "create",
        "delete",
        "deletecollection",
        "get",
        "list",
        "patch",
        "update",
        "watch"
      ],
      "shortNames": [
        "rc"
      ],
      "categories": [
        "all"
      ],
      "storageVersionHash": "Jond2If31h0="
    },
    {
      "name": "replicationcontrollers/scale",
      "singularName": "",
      "namespaced": true,
      "group": "autoscaling",
      "version": "v1",
      "kind": "Scale",
      "verbs": [
        "get",
        "patch",
        "update"
      ]
    },
    {
      "name": "replicationcontrollers/status",
      "singularName": "",
      "namespaced": true,
      "kind": "ReplicationController",
      "verbs": [
        "get",
        "patch",
        "update"
      ]
    },
    {
      "name": "resourcequotas",
      "singularName": "resourcequota",
      "namespaced": true,
      "kind": "ResourceQuota",
      "verbs": [
        "create",
        "delete",
        "deletecollection",
        "get",
        "list",
        "patch",
        "update",
        "watch"
      ],
      "shortNames": [
        "quota"
      ],
      "storageVersionHash": "8uhSgffRX6w="
    },
    {
      "name": "resourcequotas/status",
      "singularName": "",
      "namespaced": true,
      "kind": "ResourceQuota",
      "verbs": [
        "get",
        "patch",
        "update"
      ]
    },
    {
      "name": "secrets",
      "singularName": "secret",
      "namespaced": true,
      "kind": "Secret",
      "verbs": [
        "create",
        "delete",
        "deletecollection",
        "get",
        "list",
        "patch",
        "update",
        "watch"
      ],
      "storageVersionHash": "S6u1pOWzb84="
    },
    {
      "name": "serviceaccounts",
      "singularName": "serviceaccount",
      "namespaced": true,
      "kind": "ServiceAccount",
      "verbs": [
        "create",
        "delete",
        "deletecollection",
        "get",
        "list",
        "patch",
        "update",
        "watch"
      ],
      "shortNames": [
        "sa"
      ],
      "storageVersionHash": "pbx9ZvyFpBE="
    },
    {
      "name": "serviceaccounts/token",
      "singularName": "",
      "namespaced": true,
      "group": "authentication.k8s.io",
      "version": "v1",
      "kind": "TokenRequest",
      "verbs": [
        "create"
      ]
    },
    {
      "name": "services",
      "singularName": "service",
      "namespaced": true,
      "kind": "Service",
      "verbs": [
        "create",
        "delete",
        "deletecollection",
        "get",
        "list",
        "patch",
        "update",
        "watch"
      ],
      "shortNames": [
        "svc"
      ],
      "categories": [
        "all"
      ],
      "storageVersionHash": "0/CO1lhkEBI="
    },
    {
      "name": "services/proxy",
      "singularName": "",
      "namespaced": true,
      "kind": "ServiceProxyOptions",
      "verbs": [
        "create",
        "delete",
        "get",
        "patch",
        "update"
      ]
    },
    {
      "name": "services/status",
      "singularName": "",
      "namespaced": true,
      "kind": "Service",
      "verbs": [
        "get",
        "patch",
        "update"
      ]
    }
  ]
}/ # curl -H "Authorization: Bearer $TOKEN" \
> https://kubernetes/api/v1/namespaces/default/pods/ --insecure
{
  "kind": "Status",
  "apiVersion": "v1",
  "metadata": {},
  "status": "Failure",
  "message": "pods is forbidden: User \"system:serviceaccount:default:default\" cannot list resource \"pods\" in API group \"\" in the namespace \"default\"",
  "reason": "Forbidden",
  "details": {
    "kind": "pods"
  },
  "code": 403
}/ # / # curl -H "Authorization: Bearer $TOKEN" https://kubernetes/api/v1/ --insecure
sh: /: Permission denied
/ #  curl -H "Authorization: Bearer $TOKEN" https://kubernetes/api/v1/ --insecure
{
  "kind": "APIResourceList",
  "groupVersion": "v1",
  "resources": [
    {
      "name": "bindings",
      "singularName": "binding",
      "namespaced": true,
      "kind": "Binding",
      "verbs": [
        "create"
      ]
    },
    {
      "name": "componentstatuses",
      "singularName": "componentstatus",
      "namespaced": false,
      "kind": "ComponentStatus",
      "verbs": [
        "get",
        "list"
      ],
      "shortNames": [
        "cs"
      ]
    },
    {
      "name": "configmaps",
      "singularName": "configmap",
      "namespaced": true,
      "kind": "ConfigMap",
      "verbs": [
        "create",
        "delete",
        "deletecollection",
        "get",
        "list",
        "patch",
        "update",
        "watch"
      ],
      "shortNames": [
        "cm"
      ],
      "storageVersionHash": "qFsyl6wFWjQ="
    },
    {
      "name": "endpoints",
      "singularName": "endpoints",
      "namespaced": true,
      "kind": "Endpoints",
      "verbs": [
        "create",
        "delete",
        "deletecollection",
        "get",
        "list",
        "patch",
        "update",
        "watch"
      ],
      "shortNames": [
        "ep"
      ],
      "storageVersionHash": "fWeeMqaN/OA="
    },
    {
      "name": "events",
      "singularName": "event",
      "namespaced": true,
      "kind": "Event",
      "verbs": [
        "create",
        "delete",
        "deletecollection",
        "get",
        "list",
        "patch",
        "update",
        "watch"
      ],
      "shortNames": [
        "ev"
      ],
      "storageVersionHash": "r2yiGXH7wu8="
    },
    {
      "name": "limitranges",
      "singularName": "limitrange",
      "namespaced": true,
      "kind": "LimitRange",
      "verbs": [
        "create",
        "delete",
        "deletecollection",
        "get",
        "list",
        "patch",
        "update",
        "watch"
      ],
      "shortNames": [
        "limits"
      ],
      "storageVersionHash": "EBKMFVe6cwo="
    },
    {
      "name": "namespaces",
      "singularName": "namespace",
      "namespaced": false,
      "kind": "Namespace",
      "verbs": [
        "create",
        "delete",
        "get",
        "list",
        "patch",
        "update",
        "watch"
      ],
      "shortNames": [
        "ns"
      ],
      "storageVersionHash": "Q3oi5N2YM8M="
    },
    {
      "name": "namespaces/finalize",
      "singularName": "",
      "namespaced": false,
      "kind": "Namespace",
      "verbs": [
        "update"
      ]
    },
    {
      "name": "namespaces/status",
      "singularName": "",
      "namespaced": false,
      "kind": "Namespace",
      "verbs": [
        "get",
        "patch",
        "update"
      ]
    },
    {
      "name": "nodes",
      "singularName": "node",
      "namespaced": false,
      "kind": "Node",
      "verbs": [
        "create",
        "delete",
        "deletecollection",
        "get",
        "list",
        "patch",
        "update",
        "watch"
      ],
      "shortNames": [
        "no"
      ],
      "storageVersionHash": "XwShjMxG9Fs="
    },
    {
      "name": "nodes/proxy",
      "singularName": "",
      "namespaced": false,
      "kind": "NodeProxyOptions",
      "verbs": [
        "create",
        "delete",
        "get",
        "patch",
        "update"
      ]
    },
    {
      "name": "nodes/status",
      "singularName": "",
      "namespaced": false,
      "kind": "Node",
      "verbs": [
        "get",
        "patch",
        "update"
      ]
    },
    {
      "name": "persistentvolumeclaims",
      "singularName": "persistentvolumeclaim",
      "namespaced": true,
      "kind": "PersistentVolumeClaim",
      "verbs": [
        "create",
        "delete",
        "deletecollection",
        "get",
        "list",
        "patch",
        "update",
        "watch"
      ],
      "shortNames": [
        "pvc"
      ],
      "storageVersionHash": "QWTyNDq0dC4="
    },
    {
      "name": "persistentvolumeclaims/status",
      "singularName": "",
      "namespaced": true,
      "kind": "PersistentVolumeClaim",
      "verbs": [
        "get",
        "patch",
        "update"
      ]
    },
    {
      "name": "persistentvolumes",
      "singularName": "persistentvolume",
      "namespaced": false,
      "kind": "PersistentVolume",
      "verbs": [
        "create",
        "delete",
        "deletecollection",
        "get",
        "list",
        "patch",
        "update",
        "watch"
      ],
      "shortNames": [
        "pv"
      ],
      "storageVersionHash": "HN/zwEC+JgM="
    },
    {
      "name": "persistentvolumes/status",
      "singularName": "",
      "namespaced": false,
      "kind": "PersistentVolume",
      "verbs": [
        "get",
        "patch",
        "update"
      ]
    },
    {
      "name": "pods",
      "singularName": "pod",
      "namespaced": true,
      "kind": "Pod",
      "verbs": [
        "create",
        "delete",
        "deletecollection",
        "get",
        "list",
        "patch",
        "update",
        "watch"
      ],
      "shortNames": [
        "po"
      ],
      "categories": [
        "all"
      ],
      "storageVersionHash": "xPOwRZ+Yhw8="
    },
    {
      "name": "pods/attach",
      "singularName": "",
      "namespaced": true,
      "kind": "PodAttachOptions",
      "verbs": [
        "create",
        "get"
      ]
    },
    {
      "name": "pods/binding",
      "singularName": "",
      "namespaced": true,
      "kind": "Binding",
      "verbs": [
        "create"
      ]
    },
    {
      "name": "pods/ephemeralcontainers",
      "singularName": "",
      "namespaced": true,
      "kind": "Pod",
      "verbs": [
        "get",
        "patch",
        "update"
      ]
    },
    {
      "name": "pods/eviction",
      "singularName": "",
      "namespaced": true,
      "group": "policy",
      "version": "v1",
      "kind": "Eviction",
      "verbs": [
        "create"
      ]
    },
    {
      "name": "pods/exec",
      "singularName": "",
      "namespaced": true,
      "kind": "PodExecOptions",
      "verbs": [
        "create",
        "get"
      ]
    },
    {
      "name": "pods/log",
      "singularName": "",
      "namespaced": true,
      "kind": "Pod",
      "verbs": [
        "get"
      ]
    },
    {
      "name": "pods/portforward",
      "singularName": "",
      "namespaced": true,
      "kind": "PodPortForwardOptions",
      "verbs": [
        "create",
        "get"
      ]
    },
    {
      "name": "pods/proxy",
      "singularName": "",
      "namespaced": true,
      "kind": "PodProxyOptions",
      "verbs": [
        "create",
        "delete",
        "get",
        "patch",
        "update"
      ]
    },
    {
      "name": "pods/resize",
      "singularName": "",
      "namespaced": true,
      "kind": "Pod",
      "verbs": [
        "get",
        "patch",
        "update"
      ]
    },
    {
      "name": "pods/status",
      "singularName": "",
      "namespaced": true,
      "kind": "Pod",
      "verbs": [
        "get",
        "patch",
        "update"
      ]
    },
    {
      "name": "podtemplates",
      "singularName": "podtemplate",
      "namespaced": true,
      "kind": "PodTemplate",
      "verbs": [
        "create",
        "delete",
        "deletecollection",
        "get",
        "list",
        "patch",
        "update",
        "watch"
      ],
      "storageVersionHash": "LIXB2x4IFpk="
    },
    {
      "name": "replicationcontrollers",
      "singularName": "replicationcontroller",
      "namespaced": true,
      "kind": "ReplicationController",
      "verbs": [
        "create",
        "delete",
        "deletecollection",
        "get",
        "list",
        "patch",
        "update",
        "watch"
      ],
      "shortNames": [
        "rc"
      ],
      "categories": [
        "all"
      ],
      "storageVersionHash": "Jond2If31h0="
    },
    {
      "name": "replicationcontrollers/scale",
      "singularName": "",
      "namespaced": true,
      "group": "autoscaling",
      "version": "v1",
      "kind": "Scale",
      "verbs": [
        "get",
        "patch",
        "update"
      ]
    },
    {
      "name": "replicationcontrollers/status",
      "singularName": "",
      "namespaced": true,
      "kind": "ReplicationController",
      "verbs": [
        "get",
        "patch",
        "update"
      ]
    },
    {
      "name": "resourcequotas",
      "singularName": "resourcequota",
      "namespaced": true,
      "kind": "ResourceQuota",
      "verbs": [
        "create",
        "delete",
        "deletecollection",
        "get",
        "list",
        "patch",
        "update",
        "watch"
      ],
      "shortNames": [
        "quota"
      ],
      "storageVersionHash": "8uhSgffRX6w="
    },
    {
      "name": "resourcequotas/status",
      "singularName": "",
      "namespaced": true,
      "kind": "ResourceQuota",
      "verbs": [
        "get",
        "patch",
        "update"
      ]
    },
    {
      "name": "secrets",
      "singularName": "secret",
      "namespaced": true,
      "kind": "Secret",
      "verbs": [
        "create",
        "delete",
        "deletecollection",
        "get",
        "list",
        "patch",
        "update",
        "watch"
      ],
      "storageVersionHash": "S6u1pOWzb84="
    },
    {
      "name": "serviceaccounts",
      "singularName": "serviceaccount",
      "namespaced": true,
      "kind": "ServiceAccount",
      "verbs": [
        "create",
        "delete",
        "deletecollection",
        "get",
        "list",
        "patch",
        "update",
        "watch"
      ],
      "shortNames": [
        "sa"
      ],
      "storageVersionHash": "pbx9ZvyFpBE="
    },
    {
      "name": "serviceaccounts/token",
      "singularName": "",
      "namespaced": true,
      "group": "authentication.k8s.io",
      "version": "v1",
      "kind": "TokenRequest",
      "verbs": [
        "create"
      ]
    },
    {
      "name": "services",
      "singularName": "service",
      "namespaced": true,
      "kind": "Service",
      "verbs": [
        "create",
        "delete",
        "deletecollection",
        "get",
        "list",
        "patch",
        "update",
        "watch"
      ],
      "shortNames": [
        "svc"
      ],
      "categories": [
        "all"
      ],
      "storageVersionHash": "0/CO1lhkEBI="
    },
    {
      "name": "services/proxy",
      "singularName": "",
      "namespaced": true,
      "kind": "ServiceProxyOptions",
      "verbs": [
        "create",
        "delete",
        "get",
        "patch",
        "update"
      ]
    },
    {
      "name": "services/status",
      "singularName": "",
      "namespaced": true,
      "kind": "Service",
      "verbs": [
        "get",
        "patch",
        "update"
      ]
    }
  ]
/ # command terminated with exit code 137
