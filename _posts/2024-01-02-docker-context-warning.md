---
title: "Unable to resolve Docker CLI context default"
date: "2024-01-02 09:00:00 +0900"
categories:
  - kubernetes
tags:
  - minikube
  - docker desktop
toc: false
---
[Docker Desktop](https://www.docker.com/products/docker-desktop/)을 기반으로 [minikube](https://minikube.sigs.k8s.io/)를 사용해서 로컬 클러스터를 구성한 경우에,
다음 경고가 출력될 수 있다.

```
C:\> minikube start
... Unable to resolve the current Docker CLI context "default": context "default": context not found
...
```

"default" context를 사용하지 않음으로써 발생하는 경고입니다. docker context use 명령으로 "default" context를 사용하도록 설정합니다.
```
C:\> docker context use default
default
Current context is now "default"
```

---

<figure>
  <a href="https://inf.run/1zjZ">
  <img src="/assets/images/k8s-beyond/kub101-ad.png" style="background-color:#43487C"
     alt="인프런 - 쿠버네티스 101 - 클라우드/서버 개발 첫걸음"></a>
</figure>
