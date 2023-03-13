# 수동 Refresh

## 개념

3분 주기를 기다리지 않고 사용자가 수동으로 Refresh하는 방법이 존재합니다. git에 업데이된 내용을 바로 쿠버네티스 클러스터에 적용하고 싶을 떄 사용합니다.



## Refresh 종류

refresh는 일반 refresh와 hard refresh가 있습니다. hard refresh는 캐시를 무효화시키고 refresh를 수행합니다. 아마도 argocd controller가 내부적으로 캐시 매커니즘을 사용하는 것 같아요.

{% hint style="info" %}
참고자료: [https://github.com/argoproj/argo-cd/discussions/8260#discussioncomment-2031106](https://github.com/argoproj/argo-cd/discussions/8260#discussioncomment-2031106)
{% endhint %}

{% hint style="info" %}
참고자료: [https://argo-cd.readthedocs.io/en/stable/core\_concepts/](https://argo-cd.readthedocs.io/en/stable/core\_concepts/)
{% endhint %}

{% hint style="info" %}
참고자료: [https://argo-cd.readthedocs.io/en/stable/user-guide/commands/argocd\_app\_get/](https://argo-cd.readthedocs.io/en/stable/user-guide/commands/argocd\_app\_get/)
{% endhint %}



## 수동 Refresh 실행방법

WEB UI에서는 상단 메뉴에서 \[Refresh]버튼을 클릭하면 됩니다.

![](<../.gitbook/assets/image (191).png>)



## 예제

{% hint style="info" %}
예제 실습을 위해 실습 github repo fork([undefined.md](../argo-cd/undefined.md "mention"))가 필요합니다.
{% endhint %}

fork한 repo에 있는 example-3을 기준으로 애플리케이션을 생성합니다. git주소를 제외하고는 helloworld예제)([nginx-pod-service.md](../argo-cd-helloworld/nginx-pod-service.md "mention")) 생성단계와 동일합니다.

* application 이름: example-3
* git 정보
  * repo: fork한 github 주소
  * branch: main
  * path: example-3
* kubernetes
  * namespace: default



application 상세페이지에서 \[Sync]버튼을 클릭해서 동기화를 해줍니다. 동기화가 잘되었다면 왼쪽 위에 Healthy라고 표시됩니다.

![](<../.gitbook/assets/image (195).png>)

![](<../.gitbook/assets/image (124).png>)



github에 있는 example-3폴더의 deployment.yaml을 수정해볼게요. replica갯수를 현재 설정된 갯수가 아닌 다른 갯수로 변경합니다. 그리고 git push를 하세요

argocd로 돌아와 상댄메뉴에 있는 \[Refresh]버튼을 클릭합니다. 몇 초 뒤에 OutOfSync라는 문자열이 보이면서 주황색 아이콘이 표시됩니다. git과 현재 클러스터 정보가 달라, argocd가 의도된 상태와 현재 생태가 다르다고 표현한 겁니다.

![](<../.gitbook/assets/image (179).png>)



git에 push된 의도된 상태가 올바르지 않다면(예: 쿠버네티스 문법 오류, namespace 없음) Error메세지가 표시됩니다.

![](<../.gitbook/assets/image (209).png>)













