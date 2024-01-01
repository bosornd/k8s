---
title: "Docker Desktop 오류"
date: "2024-01-01 09:00:00 +0900"
categories:
  - kubernetes
tags:
  - docker
  - wsl error
toc: false
---
[Docker Desktop](https://www.docker.com/products/docker-desktop/) 실행 시,
다음과 같은 Unexpected WSL Error가 발생할 수 있습니다.

<figure>
  <a href="/assets/images/docker-desktop/error.png">
  <img src="/assets/images/docker-desktop/error.png" alt="WSL 오류"></a>
</figure>

Docker Desktop은 HW 가상화 솔루션을 사용하는데, BIOS에서 가상화가 비활성화 되어 있는 경우에도 상기 오류가 발생합니다.

* Intel CPU의 경우, Intel (VMX) Virtualization Technology를 활성화 해야 합니다.

<figure>
  <a href="/assets/images/docker-desktop/bios-enable-virtualization.jpg">
  <img src="/assets/images/docker-desktop/bios-enable-virtualization.jpg" alt="Intel VMX 활성화"></a>
</figure>

* AMD CPU의 경우, SVM(Secure Virtual Machine)을 활성화 해야 합니다.

<figure>
  <a href="/assets/images/docker-desktop/svm.gif">
  <img src="/assets/images/docker-desktop/svm.gif" alt="AMD SVM 활성화"></a>
</figure>

먼저 BIOS 설정을 확인해 보시기 바랍니다.

---

<figure>
  <a href="https://inf.run/1zjZ">
  <img src="/assets/images/k8s-beyond/kub101-ad.png" style="background-color:#43487C"
     alt="인프런 - 쿠버네티스 101 - 클라우드/서버 개발 첫걸음"></a>
</figure>
