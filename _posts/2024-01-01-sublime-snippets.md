---
title: "Sublime Text - Kubernetes snippets 사용하기"
date: "2024-01-01 12:00:00 +0900"
categories:
  - kubernetes
tags:
  - sublime text
  - snippets
toc: false
---
Sublime Text 편집기에 [Kubernetes YAML Snippets](https://github.com/bosornd/sublime-kubernetes-snippets)을 설치합니다.
Sublime Text는 <b>%appdata%\Sublime Text\Packages\User</b> 폴더에 스니펫을 설치합니다.

<figure>
  <a href="/assets/images/snippets/sublime1.png">
  <img src="/assets/images/snippets/sublime1.png" alt="Sublime Text 확장 폴더"></a>
</figure>

https://github.com/bosornd/sublime-kubernetes-snippets을 복사합니다. git clone 명령으로 복사하거나 zip 파일을 다운받아서 압축을 풀면 됩니다.

<figure>
  <a href="/assets/images/snippets/sublime2.png">
  <img src="/assets/images/snippets/sublime2.png" alt="Sublime Text 스니펫 설치"></a>
</figure>
<figure>
  <a href="/assets/images/snippets/sublime3.png">
  <img src="/assets/images/snippets/sublime3.png" alt="Sublime Text 스니펫 설치"></a>
</figure>

다음과 같이 YAML 파일에서 쿠버네티스 오브젝트를 쉽게 작성할 수 있습니다. deploy를 입력하고, Deployment 스니펫을 선택합니다.

<figure>
  <a href="/assets/images/snippets/sublime - example1.png">
  <img src="/assets/images/snippets/sublime - example1.png" alt="스니펫 사용 예제"></a>
</figure>

생성하고자 하는 Deployment 오브젝트의 이름을 입력하면, 레이블 등 관련 내용이 같이 변경됩니다. 탭을 누르면, 다음 입력 항목으로 넘어갑니다.

<figure>
  <a href="/assets/images/snippets/sublime - example2.png">
  <img src="/assets/images/snippets/sublime - example2.png" alt="스니펫 사용 예제"></a>
</figure>

---

<figure>
  <a href="https://inf.run/1zjZ">
  <img src="/assets/images/k8s-beyond/kub101-ad.png" style="background-color:#43487C"
     alt="인프런 - 쿠버네티스 101 - 클라우드/서버 개발 첫걸음"></a>
</figure>
