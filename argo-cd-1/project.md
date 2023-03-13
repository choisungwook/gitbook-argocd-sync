# Project

{% embed url="https://youtu.be/7QD6llN-WPo?t=270" %}

## Project란?

project는 argocd가 관리하는 논리적 그룹이고 application을 관리합니다. <mark style="color:red;">사용자가 생성한 application은 무조건 project에 속합니다.</mark>

![](<../.gitbook/assets/image (208).png>)



helloworld 예제([nginx-pod-service.md](../argo-cd-helloworld/nginx-pod-service.md "mention"))를 실습할 때는 default project를 선택했습니다. argocd를 설치하면 쿠버네티스 default namespace처럼 default project가 존재합니다. 그러므로 별도로 project를 생성하지 않아도 appliaction을 생성할 수 있었습니다



helloworld예제를 보면 ②과정에서 default project를 선택했었습니다.

![](<../.gitbook/assets/image (207).png>)

## Project 조회&#x20;

kubectl로는 get AppProject로 project를 조회할 수 있습니다. AppProject는 CRD로 argocd가 설치될 때 자동으로 생성됩니다.

```bash
kubectl -n argocd get AppProject
```

![](<../.gitbook/assets/image (42).png>)

WEB UI에서는 왼쪽 2번쩨 매뉴에서 확인할 수 있습니다.

![](<../.gitbook/assets/image (112).png>)

![](<../.gitbook/assets/image (199).png>)



## Project 생성

project생성은 다른 설정이 함께 동반되어서 이번 글에서는 다루지 않고 다른 글에서 다룰 예정입니다.
