---
layout: post
title: "解決 kubernetes dashboard - tls: oversized record received with length 20527 的錯誤"
subtitle: "tls: oversized record received with length 20527"
date: 2019-06-21 18:00:00 +0800
author: "cain19811028"
tags:
  - Kubernetes
---

### 前言：

以往在 MAC 上面要開 Kubernetes Dashboard 通常就是使用 `kubectl proxy`，然後連線到 [http://localhost:8001/api/v1/namespaces/kube-system/services/https:kubernetes-dashboard:/proxy/](http://localhost:8001/api/v1/namespaces/kube-system/services/https:kubernetes-dashboard:/proxy/) 就能看到美美的操作介面。不過今天不知道是不是改壞了什麼，在畫面上一直跳出底下的錯誤訊息！？

```
Error: 'tls: oversized record received with length 20527'
Trying to reach: 'https://10.244.0.7:9090/'
```

上網 Google 了一下，連結改為 [http://localhost:8001/api/v1/namespaces/kube-system/services/kubernetes-dashboard/proxy](http://localhost:8001/api/v1/namespaces/kube-system/services/kubernetes-dashboard/proxy) 就能正常使用！

### MAC 使用 Kubernetes Dashboard 的方式：

 - 移除之前的安裝
```
kubectl delete -f https://raw.githubusercontent.com/kubernetes/dashboard/master/aio/deploy/recommended/kubernetes-dashboard.yaml
```

 - 重新安裝 Kubernetes Dashboard
```
kubectl create -f https://raw.githubusercontent.com/kubernetes/dashboard/master/aio/deploy/recommended/kubernetes-dashboard.yaml
```

 - 取得 Token
```
kubectl -n kube-system describe $(kubectl -n kube-system get secret -n kube-system -o name | grep namespace) | grep token
```

 - 開啟 kubectl proxy
```
kubectl proxy
```

 - 開啟 Kubernetes Dashboard
```
http://localhost:8001/api/v1/namespaces/kube-system/services/kubernetes-dashboard/proxy
```

### 相關連結：

 - [https://github.com/moby/moby/issues/37102](https://github.com/moby/moby/issues/37102)
