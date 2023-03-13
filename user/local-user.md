# Local user 생성

## Local user 관리 방법

local user는 쿠버네티스 configmap으로 관리됩니다. configmap이름은 argocd-cm입니다. 처음 설치하면 configmap에 아무런 데이터가 없습니다.

```shell
kubectl -n argocd get configmap argocd-cm
```

<figure><img src="../.gitbook/assets/image (137).png" alt=""><figcaption></figcaption></figure>

## User 생성

argocd-cm configmap에서 <mark style="color:red;">account.<계정이름>필드를 추가</mark>하면 local user이 추가됩니다. 그리고 생성한 user의 인증 방법을 설정해야 합니다. admin 사용자는 login방법만 설정되어 있습니다.

* apiKey방법은 JWT토큰 인증을 허용합니다.&#x20;
* login방법은 WEB UI 인증을 허용합니다.

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: argocd-cm
  namespace: argocd
  labels:
    app.kubernetes.io/name: argocd-cm
    app.kubernetes.io/part-of: argocd
data:
  accounts.test: apiKey, login
```



수정한 configmap을 적용하면 계정이 생성됩니다.

```
kubectl apply -f new_user.yaml
```

<figure><img src="../.gitbook/assets/image (188).png" alt=""><figcaption></figcaption></figure>



argocd account list명령어로 추가된 test계정을 확인할 수 있습니다.

```shell
argocd account list
```

<figure><img src="../.gitbook/assets/image (20).png" alt=""><figcaption></figcaption></figure>



WEB UI에서도 추가한 test user를 확인할 수 있습니다.

<figure><img src="../.gitbook/assets/image (113).png" alt=""><figcaption></figcaption></figure>



## 비밀번호 생성

생성한 test사용자을 사용하려면 비밀번호를 생성해야 합니다. 사용자 생성하면 비밀번호가 없습니다. 비밀번호를 생성하려면 argocd login명령어로 로그인한 사용자 비밀번호가 필요합니다.



아래 예제는 비밀번호를 password1234로 설정했습니다.

```shell
argocd account update-password \
--account test \
--current-password <login한 user의 비밀번호> \
--new-password "password1234"
```

<figure><img src="../.gitbook/assets/image (123).png" alt=""><figcaption></figcaption></figure>

비밀번호를 생성했으니 Argo CD WEB UI에 로그인할 수 있습니다. 물론, test계정 생성시 login방법을 설정해야 WEB UI에 로그인할 수 있습니다.

<figure><img src="../.gitbook/assets/image (128).png" alt=""><figcaption></figcaption></figure>

test사용자는 아직 권한 설정이 아무것도 되어 있지 않으므로, Permission denied메세지를 볼 수 있습니다.

<figure><img src="../.gitbook/assets/image (125).png" alt=""><figcaption></figcaption></figure>





## 참고자료

* [https://argo-cd.readthedocs.io/en/stable/operator-manual/user-management/#create-new-user](https://argo-cd.readthedocs.io/en/stable/operator-manual/user-management/#create-new-user)
* [https://rtfm.co.ua/en/argocd-users-access-and-rbac/](https://rtfm.co.ua/en/argocd-users-access-and-rbac/)

