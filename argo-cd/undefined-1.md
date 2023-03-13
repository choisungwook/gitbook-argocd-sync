# 컨셉과 장단점

{% embed url="https://youtu.be/cLgzqc_hwIg?t=1" %}

## 컨셉

ArgoCD는 공식문서 소개처럼 **쿠버네티스를 위한 GitOps도구**입니다.

![출처: https://argo-cd.readthedocs.io/en/stable/](<../.gitbook/assets/image (98).png>)



GitOps는 Git과 Operations의 합성어인데요! **현재 쿠버네티스 상태를 git으로 관리하는 문화**입니다.즉, 쿠버네티스에 배포하려고 하는 상태를 Git에 저장하면 ArgoCD가 git에 있는 내용을 쿠버네티스 배포합니다.

![](<../.gitbook/assets/image (148).png>)

****

**git에 저장되어 있는 내용은 쿠버네티스 사용자가 의도된 상태(Desired state)로 취급**됩니다. 쿠버네티스 declarative와 동일한 개념입니다.

ArgoCD는 의도된 상태를 쿠버네티스에 동기화하려면 **조건이 필요**합니다. 현재 상태와 의도된 상태와 차이가 있어야 합니다. 예를 들어 쿠버네티스에는 helloworld service가 없는데 git에는 service가 있는 상황입니다. 차이가 발생하면 ArgoCD는 의도된 상태를 쿠버네티스에 동기화를 수행합니다.

![](<../.gitbook/assets/image (76).png>)

## 장단점

GitOps의 장점을 가져갈 수 있습니다. **쿠버네티스로 관리하는 리소스를 git에 저장함으로써 코드로 관리할 수 있습니다. IAC처럼 말이죠**. 하지만, GitOps를 하려면 팀의 문화가 갖추어져야 합니다. 누구는 GitOps를 쓰고 누구는 수동으로 kubectl로 관리하면 엉망진창이 됩니다.



<mark style="color:red;">**최대 단점은 쿠버네티스에서만 ArgoCD를 사용**</mark>**할 수 있습니다**. 쿠버네티스가 아닌 다른 환경에서는 사용하지 못합니다. 혼합환경을 사용하고 있는 곳이라면 Argo CD는 적절하지 않습니다. Jenkins, CircleCI등을 고려해야 합니다. 그리고, 쿠버네티스 메커니즘을 이용하므로 쿠버네티스에 대한 지식이 필요합니다.



<mark style="color:red;">두번째 단점은 git서비스의 장애를 대비</mark>해야 합니다. public github을 사용한다고 가정하면 github 서비스가 장애난 경우 argocd는 새로운 업데이트 내용을 가져오지 못합니다. 설치용 git을 사용한다면 HPA, DA등을 고려해야 합니다.



## 지원기능&#x20;

정말 많은 기능을 지원합니다. Argo CD가 많은 사랑을 받는 것은 SSO, 도구(helm, kustomize 등) 호환성, 멀티 쿠버네티스 클러스터 관리, 사용자(또는 그룹)별 권한관리, WEB UI 제공이라고 생각합니다

![출처: https://argo-cd.readthedocs.io/en/stable/#features](<../.gitbook/assets/image (163).png>)
