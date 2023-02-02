---
title: "Kubernetes and beyond"
date: "2023-01-26 09:00:00 +0900"
categories:
  - kubernetes
tags:
  - kubernetes
  - msa
  - microservice architecture
---
* [쿠버네티스(Kubernetes, k8s)](https://kubernetes.io/)는 컨테이너 기반의 클라우드 환경을 관리하는 표준 도구입니다.
* 간단한 예제로 마이크로 서비스 구조를 쿠버네티스 환경에서 실현하는 방법에 대해서 설명합니다.
* 구현된 코드와 실행 방법은 다음 github 저장소에 있습니다.
  - [https://github.com/bosornd/k8s-example](https://github.com/bosornd/k8s-example)

## 최종 구조
* Redis DB에 저장된 count 값을 서비스 하는 counter 앱을 개발합니다.
* 그림과 같이 3개의 마이크로 서비스로 구성됩니다.

<figure>
  <a href="/assets/images/k8s-beyond/final.png">
  <img src="/assets/images/k8s-beyond/final.png" alt="최종 구조"></a>
</figure>
* 현재 count 값을 제공하는 web 서비스
* count 값을 증가시키는 web-inc 서비스
* count 값을 저장하는 redis 서비스
* 요청을 재전송하는 ingress API gateway

## 배포 구조
<figure>
  <a href="/assets/images/k8s-beyond/deployment.png">
  <img src="/assets/images/k8s-beyond/deployment.png" alt="배포 구조"></a>
</figure>
* 상태를 저장할 필요가 없는 web, web-inc 서비스는 쿠버네티스의 deployment로 배포합니다.
  - replicaset으로 pod의 개수(scale)를 쉽게 조절할 수 있습니다.
  - auto-scaler로 pod의 개수를 자동으로 조정할 수도 있습니다.
  - rolling update로 서비스 중단 없이 이미지를 변경합니다.
* 쿠버네티스 service는 pod를 노출하고 요청을 분배합니다. &larr; Load Balancer.
* 상태 저장이 필요한 redis 서비스는 쿠버네티스의 statefulset으로 배포합니다.
  - statefulset은 deployment와 마찬가지로 pod를 생성하고 관리합니다.
  - deployment와 다르게, 고정된 이름(상태)을 가진 pod가 생성됩니다.
  - DNS에도 등록됩니다. 따라서, pod에 직접 기능을 요청하는 것이 가능합니다.
  - pod 마다 persistent volume이 할당되어 데이터(상태)를 저장할 수 있습니다.
* Redis DB를 master-slave 구조로 구성해서 동일한 데이터를 서비스하도록 합니다.
  - statefulset이 상태를 저장할 수 있는 저장 공간을 제공하지만, 데이터를 일치시키는 것은 아닙니다.
  - redis-0을 master로, 나머지 redis pod는 slave로 하는 master-slave 구조로 구성합니다.
  - 변경 명령은 redis-0으로 요청합니다. redis-0가 slave에게 메시지를 보내서 데이터를 일치시킵니다.
  - 질의는 redis 서비스로 요청합니다. 복수 Redis DB가 처리하므로 성능 제고를 기대할 수 있습니다.
  - 이처럼 변경 명령과 질의를 구분해서 처리하는 것이 CQRS(Command and Query Responsibility Segregation) 패턴입니다.
* ingress API gateway로 단일 진입점을 생성합니다.
  - /inc의 요청은 web-inc 서비스로 전달합니다.
  - 나머지 요청은 web 서비스로 전달합니다.
  - 내부 서비스 구성을 외부에서는 알 필요가 없습니다.
    즉, 내부 서비스 구성이 바뀌더라도 외부에는 영향을 주지 않습니다.

## 모노리틱 구조
* 그럼, web과 web-inc로 서비스를 구분하면 장점은 무엇일까요? 단점은?
* 서비스를 나누지 않았다면 다음 그림과 같이 web 서비스에서 GET 요청도,
INC 요청도 처리할 것입니다.

<figure>
  <a href="/assets/images/k8s-beyond/final2.png">
  <img src="/assets/images/k8s-beyond/final2.png" alt="모노리틱 구조"></a>
</figure>

* 서비스가 구분되지 않은 경우
  - GET 요청이 빈번할 경우, INC 요청이 지연될 수 있습니다. 요청이 순차적으로 처리되기 때문입니다.
* 서비스가 구분된 경우
  - INC 요청이 web-inc 서비스에서 처리되므로 web 서비스의 부하에 영향을 받지 않습니다.
  - web 서비스 부하가 심한 경우에, web 서비스 부하에 따라 pod의 개수를 증설할 수 있습니다.
  - web-inc 서비스의 오류나 변경이 필요한 경우에, web 서비스에는 영향을 주지 않습니다.

이처럼 독립된 서비스는 서로 영향을 미치지 않기 때문에 유지보수 측면에서 장점이 많습니다.
반면 서비스를 구분하는 경우에 요구되는 pod나 컨테이너의 수가 늘어나기 쉽습니다. 비용이 증가됩니다.
또한, 각각의 서비스는 작고 단순하지만 전체적으로는 매우 복잡한 구조가 되기 싶습니다.

마이크로 서비스 구조로 설계하고 개발할 때는 이런 특성을 충분히 이해하고 반영해야 합니다.
어떤 서비스를 구분하는 것이 최적인지를 검토해야 합니다.

## 모듈 구조
구현된 소스 코드, 모듈 구조는 다음과 같습니다. 화살표는 의존성입니다.
<figure>
  <a href="/assets/images/k8s-beyond/module.png">
  <img src="/assets/images/k8s-beyond/module.png" alt="모듈 구조"></a>
</figure>

* 인터페이스 ICountDB는 CountDB의 인터페이스입니다.
  - CountDB가 ICountDB를 구현합니다.
  - logic.ts는 ICountDB를 사용합니다.
* config.ts에서 ICountDB의 구현을 설정합니다. &larr; Dependency Injection.
  - logic.ts가 CountDB를 의존하지 않도록 합니다.
* server.ts는 logic.ts가 제공하는 router로 웹 서버를 생성합니다.

## 모듈 구조2
모듈 구조를 다음과 같이 수정하면 어떨까요?
<figure>
  <a href="/assets/images/k8s-beyond/module2.png">
  <img src="/assets/images/k8s-beyond/module2.png" alt="모듈 구조2"></a>
</figure>

* 모듈 구조에서는 ICountDB 인터페이스가 data 패키지에 CountDB와 함께 있고,
  모듈 구조2에서는 ICountDB가 logic 패키지에 logic.ts와 함께 있습니다.
  - 모듈 구조에서 ICountDB 인터페이스는 CountDB가 제공하는 인터페이스의 맥락으로 관리됩니다.
    따라서, CountDB가 변경되는 경우에 ICountDB 인터페이스가 변경될 수 있습니다.
    이는 logic.ts에도 영향을 미칠 수 있습니다.
  - 모듈 구조2에서는 ICountDB 인터페이스가 logic.ts가 필요로하는 인터페이스의 맥락으로 관리됩니다.
    따라서, CountDB의 구현이 변경되더라도 logic.ts의 요구사항이 바뀌지 않는다면 영향을 받지 않을 것입니다.
  - 반면, CountDB에 대한 logic.ts의 요구사항 변경이 빈번한 경우에,
    - 모듈 구조에서는 CountDB가 제공하는 기본 인터페이스를 logic.ts에서 확장해서 사용하도록 할 것입니다.
    - 모듈 구조2에서는 ICountDB가 변경되고, CountDB가 이에 맞춰 구현을 변경할 것입니다.
* 어떤 구조가 더 좋은가는 어떤 변경이 더 빈번한지, 복잡한 지에 따라 다릅니다.
* 개발은 사람이 하는 것입니다. 누가 담당하고, 어떤 맥락에서 개발하는가를 명확히 하는 것이 좋습니다.
* 규모가 작으면 설계의 좋고 나쁨이 중요하게 느껴지지 않습니다.
  - 규모가 작건 크건, 설계 원칙, 과정 및 방법론은 유사합니다.
  - 작은 과제에서 연습하고 익혀야 아키텍트가 되는 것입니다.

<figure>
  <a href="https://inf.run/1zjZ">
  <img src="/assets/images/k8s-beyond/kub101-ad.png" style="background-color:#43487C"
     alt="인프런 - 쿠버네티스 101 - 클라우드/서버 개발 첫걸음"></a>
</figure>
