# 권한(Role) 관리

## 개념

Argo CD 권한은 역할기반(RBAC)을 따릅니다.



## Role 종류

Role은 Global Role과 Project Role로 분류됩니다. Global Role은 Argo CD 사용자 권한을 제한합니다. Project Role은 Project 권한을 제한합니다.



## Global Role

### 개념

Argo CD 사용자가 할 수 있는 행동을 제한합니다. 어떤 리소스의 행동(Action)을 허용할지 거부할지를 설정합니다.

<figure><img src="../.gitbook/assets/image (201).png" alt=""><figcaption><p>출처: <a href="https://argo-cd.readthedocs.io/en/stable/operator-manual/rbac/#rbac-resources-and-actions">https://argo-cd.readthedocs.io/en/stable/operator-manual/rbac/#rbac-resources-and-actions</a></p></figcaption></figure>



### 관리 방법

Global Role은 csv파일로 관리됩니다. csv파일은 쿠버네티스 configmap으로 관리됩니다.

```shell
kubectl -n argocd get configmap argocd-rbac-cm
```

<figure><img src="../.gitbook/assets/image (29).png" alt=""><figcaption></figcaption></figure>



role 설정은 아래 구조로 설정합니다.

```
p, role:<role이름>, <리소스 종류>, <action>, <리소스 이름>, allow/deny
```

## 예제

test이름을 갖는 Argo CD사용자가 default 프로젝의 모든 Appliction에만 권한을 갖도록 설정하겠습니다.&#x20;



Role을 생성하고, Local user 예제([local-user.md](local-user.md "mention"))에서 생성한 test사용자에게 생성한 Role을 연결시켜보습니다.

Application을 생성하려면 프로젝트를 조회할 권한과 클러스터 조회할 권한이 필요하여 projects, clusters 조회 권한을 허용했습니다. 그리고 default의 모든 appliction의 모든 권한을 허용했습니다.

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: argocd-rbac-cm
  namespace: argocd
data:
  # test-role은 모든 project의 모든 application 제어 권한만 갖는다.
  # 그리고 test사용자에 test-role을 연결시킨다. 이 설정은 global-role로 적용된다.
  policy.csv: |
    p, role:test-role, applications, *, default/*, allow
    p, role:test-role, projects, get, default, allow
    p, role:test-role, clusters, get, *, allow
    g, test, role:test-role
```



configmap을 적용하면 Role이 생성되고 test사용자에게 test-role이 설정됩니다.

```
kubectl apply -f new_global_role.yaml
```

<figure><img src="../.gitbook/assets/image (14).png" alt=""><figcaption></figcaption></figure>



Argo CD Web UI를 이용하여 test사용자 로그인을 합니다.

<figure><img src="../.gitbook/assets/image (41).png" alt=""><figcaption></figcaption></figure>



그 이후 helloworld 예제([Broken link](broken-reference "mention"))를 따라하시면 됩니다.



## Project Role

project role은 project 범위에 한정된 권한 설정입니다. project챕터에서 다룹니다.

## 참고자료

* [https://argo-cd.readthedocs.io/en/stable/operator-manual/rbac/](https://argo-cd.readthedocs.io/en/stable/operator-manual/rbac/)
* [https://rtfm.co.ua/en/argocd-users-access-and-rbac/](https://rtfm.co.ua/en/argocd-users-access-and-rbac/)
