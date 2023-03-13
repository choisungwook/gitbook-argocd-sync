# Phases

{% embed url="https://youtu.be/-T1p0uy1mxs" %}

## 개념

ArgocD sync과정 순서를 phase라고 부르며 3단계(pre sync, sync, post sync)로 진행됩니다. 지금까지 실습했던 예제들은 모드 sync단계에 속합니다. pre-sync와 post sync는 annotations필드에 설정하여 사용할 수 있습니다.

![](<../.gitbook/assets/image (151).png>)



## phase를 어떻게 활용하지?

phase를 활용하면 내부적으로 관리하는 DB업데이트, 알림기능을 구현할 수 있습니다.



지금까지는 단순히 argocd를 git의 의도된 상태를 쿠버네티스에 배포하는 기능으로만 사용했습니다. 비지니스환경에서는 관측성(Observability)을 위해 사내 DB연동, 알림 기능을 argocd와 연동하고 싶어 합니다. argocd는 각 비지니스 시스템을 알지 못하므로 저희가 직접 Sync LifeCycle을 이용하여 연계작업을 해야 합니다.



## pre sync, post sync설정 방법

sync와 다르게 pre-sync와 post sync는 사용자가 직접 설정해서 샤용해야 합니다. annotations필드에 설정하여 사용할 수 있습니다. annotations은 argocd hook필드를 사용하면 됩니다. 자세한 내용은 argocd resouce hook공식문서([https://argo-cd.readthedocs.io/en/stable/user-guide/resource\_hooks/](https://argo-cd.readthedocs.io/en/stable/user-guide/resource\_hooks/))를 참고하시길 바랍니다.



* PreSync 설정방법

```yaml
apiVersion: batch/v1
kind: Job
metadata:
  generateName: schema-migrate-
  annotations:
    argocd.argoproj.io/hook: PreSync
```



* PostSync 설정방법

```yaml
apiVersion: batch/v1
kind: Job
metadata:
  generateName: integration-test-
  annotations:
    argocd.argoproj.io/hook: PostSync
```



## 예제

argocd github에서 phase예제로 argocd application을 생성합니다.

* application 이름: phase
* git
  * repo:[https://github.com/argoproj/argocd-example-apps](https://github.com/argoproj/argocd-example-apps)
  * branch: master
  * path: pre-post-sync
* kubernetes
  * namespace: default



예제는 deployment, service를 배포하는 전/후 과정에 job을 실행합니다.

![](<../.gitbook/assets/image (80).png>)



application을 생성 후 sync하면 아래 그림과 같이 잘 동기화됩니다.

![](<../.gitbook/assets/image (83).png>)



job은 삭제정책(delete-policy)가 설정되어 있어서 argocd에서는 보이지 않습니다.

```yaml
apiVersion: batch/v1
kind: Job
metadata:
  name: after
  annotations:
    argocd.argoproj.io/hook: PostSync
    argocd.argoproj.io/hook-delete-policy: HookSucceeded
...
```



하지만, Sync 이력(Sync Status)에는 job실행 흔적이 있습니다.

![](<../.gitbook/assets/image (159).png>)
