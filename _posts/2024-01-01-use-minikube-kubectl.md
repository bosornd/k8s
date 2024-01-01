---
title: "minikube kubectl 사용하기"
date: "2024-01-01 10:00:00 +0900"
categories:
  - kubernetes
tags:
  - minikube
  - kubectl
toc: false
---
[kubectl](https://kubernetes.io/docs/reference/kubectl/)은 쿠버네티스의 CLI(Command Line Interface) 도구입니다.
kubectl은 쿠버네티스 API 서버에 연결해서 사용자의 명령을 전달합니다.
따라서, 서버와 동일한 버전의 클라이언트를 사용하는 것이 좋습니다.<BR>

[Docker Desktop](https://www.docker.com/products/docker-desktop/)과 [minikube](https://minikube.sigs.k8s.io/)를 사용해서 로컬 클러스터를 구성한 경우에,
Docker Desktop에서 kubectl을 제공합니다만, minikube의 버전과 다를 수 있습니다.

<figure>
  <a href="/assets/images/minikube-kubectl/docker - kubectl.png">
  <img src="/assets/images/minikube-kubectl/docker - kubectl.png" alt="Docker Desktop의 kubectl 버전"></a>
</figure>

minikube API 서버의 버전은 v1.28.3인데, Docker Desktop의 kubectl 버전은 v1.28.2 임을 알 수 있습니다. <BR>

그럼, kubectl 최신 버전은 어떨까요? 현재(2024년 1월 1일) kubectl 최신 버전은 v1.29.0입니다.

<figure>
  <a href="/assets/images/minikube-kubectl/kubectl latest.png">
  <img src="/assets/images/minikube-kubectl/kubectl latest.png" alt="kubectl 최신 버전"></a>
</figure>

물론 minikube API 서버 버전과 맞는 kubectl 버전을 설치해서 사용할 수도 있습니다만,
minikube를 업그레이드하는 경우에 kubectl도 업그레이드 하는 불편함이 있습니다. <BR>

minikube 로컬 클러스터를 사용하는 경우에는 minikube에서 제공하는 kubectl을 사용하는 것이 좋습니다.

<figure>
  <a href="/assets/images/minikube-kubectl/minikube - kubectl.png">
  <img src="/assets/images/minikube-kubectl/minikube - kubectl.png" alt="minikube의 kubectl 버전"></a>
</figure>

minikube kubectl -- 뒤에, kubectl 명령을 제공하면 됩니다. -- 뒤에 반드시 공백이 필요합니다. <BR>

그런데, 매번 kubectl 명령을 입력할 때마다, minikube kubectl을 입력하는 것은 번거로운 일입니다.
alias로 설정해서 사용하면 되는데, Windows 환경에서는 다음과 같이 설정합니다.

<figure>
  <a href="/assets/images/minikube-kubectl/aliases.png">
  <img src="/assets/images/minikube-kubectl/aliases.png" alt="aliases.cmd 파일"></a>
</figure>

aliases.cmd 파일에 doskey 명령으로 alias를 설정합니다. <BR>

그리고, 명령 프롬프트 설정에서 /k aliases.cmd 옵션으로 aliases.cmd 파일의 내용을 실행하도록 합니다.

<figure>
  <a href="/assets/images/minikube-kubectl/cmd - settings.png">
  <img src="/assets/images/minikube-kubectl/cmd - settings.png" alt="명령 프롬프트 설정"></a>
</figure>

Windows11의 경우, 명령 프롬프트 윈도우의 타이틀 창에서 오른쪽 버튼/설정도 변경합니다.

<figure>
  <a href="/assets/images/minikube-kubectl/cmd - settings2.png">
  <img src="/assets/images/minikube-kubectl/cmd - settings2.png" alt="명령 프롬프트 설정2"></a>
</figure>

Visual Studio Code의 터미널 설정도 수정합니다.
File/Preferences/Settings 메뉴 또는 Ctrl+,를 누르면 설정 윈도우가 활성화 됩니다.
검색에 profile: windows를 입력합니다.

<figure>
  <a href="/assets/images/minikube-kubectl/code - settings.png">
  <img src="/assets/images/minikube-kubectl/code - settings.png" alt="VS Code 설정"></a>
</figure>

터미널 기본 설정을 Command Prompt로 바꿔줍니다.
Edit in settings.json 을 클릭해서 json 파일을 다음과 같이 수정합니다.

<figure>
  <a href="/assets/images/minikube-kubectl/code - settings2.png">
  <img src="/assets/images/minikube-kubectl/code - settings2.png" alt="VS Code 설정2"></a>
</figure>

Command Prompt의 args에 aliases.cmd 파일을 실행하도록 추가합니다.
이제 minikube kubectl을 사용할 준비가 되었습니다.

---

<figure>
  <a href="https://inf.run/1zjZ">
  <img src="/assets/images/k8s-beyond/kub101-ad.png" style="background-color:#43487C"
     alt="인프런 - 쿠버네티스 101 - 클라우드/서버 개발 첫걸음"></a>
</figure>
