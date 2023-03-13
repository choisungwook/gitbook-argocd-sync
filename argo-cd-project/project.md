# Project란?

{% embed url="https://youtu.be/rx-G89XGObA" %}

## 개념

Project는 Application을 그룹으로 관리하는 개념입니다. Appliction은 생성할 때 Project를 선택해야 하고 Project는 0개 이상 Application을 가질 수 있습니다.&#x20;

Application을 그룹핑하여 공통으로 사용하는 설정을 관리하는 장점과 Applicatoin 권한을 제한하여 안정적으로 Argo CD를 운영할 수 있게 합니다. <mark style="color:red;">Argo CD를 설치하면 default Project가 기본으로 생성</mark> 되어 있습니다.

<figure><img src="../.gitbook/assets/image (47).png" alt=""><figcaption></figcaption></figure>



## 구성요소

Project는 여러가지 정보로 구성됩니다. Git repo, sync할 쿠버네티스 클러스터 정보, Role 등이 있습니다. Project에 속한 <mark style="color:red;">Application의 대부분 설정은 Project에 설정된 정보만 사용할 수 있습니다</mark>. Project에 종속성(Dependency)이 생기는 특성을 이용하여 Appliction을 그룹핑하여 관리할 수 있습니다.&#x20;



## 활용사례

애플리케이션을 목적에 따라 Project를 구성할 수 있습니다.



예를 들어 Team이름단위로 Project를 만들 수 있습니다. 또 다른 예는 배포환경(dev, stg, prd)에 따라 Project를 분리할 수 있습니다.

<figure><img src="../.gitbook/assets/image (84).png" alt=""><figcaption></figcaption></figure>
